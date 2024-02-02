---
description: 'Copied from confluence (author: Lech Groblewicz)'
---

# Price calculation

\
Original documentation: [\[IP\] Artikelpreise (EN)](https://interpneu-webweit.atlassian.net/wiki/spaces/4I/pages/1738539199/IP+Artikelpreise+EN)

Price calculation is done on fly based on the data in `ip_price`, `ip_stocks` tables or through the AdHoc EDI API depending on the context.

For search, prices are calculated during export and stored in elasticsearch for every supported scenario (AdHoc EDI is **not used** in this case).

The calculations can be seen in [https://github.com/webweit-gmbh/WebInterpneuCore/blob/dev/src/Search/Business/ElasticSearchExtension/Product/Price.php](https://github.com/webweit-gmbh/WebInterpneuCore/blob/dev/src/Search/Business/ElasticSearchExtension/Product/Price.php) for regular pricing and RRP and in [https://github.com/webweit-gmbh/WebInterpneuCore/blob/dev/src/Search/Business/ElasticSearchExtension/Product/StockExternal.php](https://github.com/webweit-gmbh/WebInterpneuCore/blob/dev/src/Search/Business/ElasticSearchExtension/Product/StockExternal.php) for external warehouse prices and stocks.

The logic for search to evaluate proper price from product is stored in elasticsearch during indexing as painless script, see [\[DEVINTER\] Elasticsearch customisations](https://webweit.atlassian.net/wiki/spaces/wwClients/pages/3706519601) .

The calculations for the frontend display and general usage in shop is done in [https://github.com/webweit-gmbh/WebInterpneuCore/blob/dev/src/Price/Business/Hierarchy/IpPriceFinder.php](https://github.com/webweit-gmbh/WebInterpneuCore/blob/dev/src/Price/Business/Hierarchy/IpPriceFinder.php) using price chain login from `Web\InterpneuCore\Price\Business\Hierarchy\PriceHandler` namespace.

### Current logic used to determine the price <a href="#current-logic-used-to-determine-the-price" id="current-logic-used-to-determine-the-price"></a>

When external warehouse is used for given product (on listing, detail page or cart) price is brought from the `ip_stocks` (the price column).

On product detail page and in the checkout, if product has available AdHoc for given customer (and AdHoc warehouse is picked in case of cart), the price is determined via AdHoc EDI API.

If the regular or RRP price is used, following logic applies:

* The data records are mapped to those described in the customer data [\[IP\] Kunden (Import) - Datenbeschreibung](https://interpneu-webweit.atlassian.net/wiki/spaces/4I/pages/1249837069)
* in the event of a currency discrepancy, the customer data record is kept [\[IP\] Kunden (Import) - Datenbeschreibung](https://interpneu-webweit.atlassian.net/wiki/spaces/4I/pages/1249837069)
  * If the price list data record contains the same key with different currency keys, the currency record that is stored in the customer data record must be used
* each data record contains 2 prices: PriceA and PriceB
  * Price A is displayed if own inventory <= 0
  * Price B is displayed if own inventory> 0
* It can happen that an item in the price list and the price group of the customer has not been assigned a price
  * In this case, the price is determined in price list 00 in the same price group,
  * e.g. from 17 03 to 00 03
  * With this fallback, all products should be assigned a price
  * If this is not the case, no price will be shown on the item as a further fallback
  * This is then also not buyable → don’t show product in the listing

### Price hierarchy <a href="#price-hierarchy" id="price-hierarchy"></a>

* In order to determine the price, the stock figures are first required
  * the price that is displayed determines the availability
* The customer master data is known from the login
  * the data is assigned to each customer according to the description [\[IP\] Kunden (Import) - Datenbeschreibung](https://interpneu-webweit.atlassian.net/wiki/spaces/4I/pages/1249837069)
* The hierarchy levels are described as follows and are valid for all client shops:

1. Hierarchy level 1: A819 - access via table name, material number. If a price >0 was found, the price determination is done.
2. Hierarchy level 2: A992 - access via table name, material number, offer group. If a price >0 was found, the price determination is done.
3. Hierarchy level 3: A927 - access via table name, material number, price list, price group. If a price > 0 was found, the price determination is done.
4. Hierarchy level 4: A927 - access via table name, material number, price list "00", price group. If a price > 0. Reason: price list 00 is the fallback price list.

### Price Import

Interpneu exports different files that contain prices which we import:

1. These files are stored in database table `ip_price`:
   1. Export `Table A927` : Contain prices for products that are stored in Internal Warehouses. Also
   2. Export `Table A911`: Contain prices for products that are stored in Industry Warehouses.
   3. Export `Table A992` AKA `Price Matrix` : Contain prices for products that are stored in Internal Warehouses.
   4. Export `Table A819`: Contain prices for products that are stored in External Warehouses or something like this. ( #ClarficationNeeded )
   5. Export `Table A503` AKA `KB Price`: Calculation Base Prices. I am not sure but I think an end-customer price should not be less than this. ( #ClarficationNeeded )
2. These files are stored in database table `ip_stocks` :
   1. Export `Table FLPM`: Contain prices and stock for products that are stored in External Warehouses.

