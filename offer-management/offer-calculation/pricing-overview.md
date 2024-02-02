# Pricing Overview

### Prices

For around 1.5 million products there are around 13 million prices. In a very broad sense the price of the product is depended on the product, debtor and warehouse. Each debtor therefore each of their contact have two main attributes called "Price List Type SAP" (`\Web\InterpneuCore\Configuration\CustomFields::CUSTOMER_PLTYP`) and "Price Group SAP" (`\Web\InterpneuCore\Configuration\CustomFields::CUSTOMER_KONDA`) that define the prices that they see. These are set in database table `customer` in column `custom_fields`.

Prices are saved in database tables `ip_price` and `ip_stocks`.

Interpneu exports different files that contain prices which we import:

1. These files are stored in database table `ip_price`:
   1. Export `Table A927` : Contain prices for products that are stored in Internal Warehouses. Also
   2. Export `Table A911`: Contain prices for products that are stored in Industry Warehouses.
   3. Export `Table A992` AKA `Price Matrix` : Contain prices for products that are stored in Internal Warehouses.
   4. Export `Table A819`: Contain prices for products that are stored in External Warehouses or something like this. ( #ClarficationNeeded )
   5. Export `Table A503` AKA `KB Price`: Calculation Base Prices. I am not sure but I think an end-customer price should not be less than this. ( #ClarficationNeeded )
2. These files are stored in database table `ip_stocks` :
   1. Export `Table FLPM`: Contain prices and stock for products that are stored in External Warehouses.

Columns of `ip_price`:

1. id: Just auto increment #ClarficationNeeded
2. table\_name: where did this price came from
3. product\_number: for which product is this price applied
4. price\_list: most of the cases the price list that debtor is part of.
   1. In case if `table_name` is A503 then it does not matter.
   2. In case If `table_name` is A927 and value starts with `E` then price is RRP price
5. product\_group: most of the cases the price list that debtor is part of.
   1. In case if `table_name` is A503 or A992 then it does not matter.
6. price\_a: price to show if a product is available
7. price\_b: price to show if a product is NOT available

### Price Calculation

{% hint style="danger" %}
This might be outdate. Look at [Pricing Calculation](pricing-calculation.md) for a better overview
{% endhint %}

The calculation of prices starts here `\Web\InterpneuCore\Price\Business\PriceCalculator\IpProductPriceCalculator::calculate` which is called on `SalesChannelEntityLoadedEvent`.

All prices that are custom-made for Interpneu are searched in `\Web\InterpneuCore\Price\Business\Hierarchy\IpPriceFinder::findList` for multiple products at once or `\Web\InterpneuCore\Price\Business\Hierarchy\IpPriceFinder::find` for one product.

Prices for product that have stock in External Warehouses are taken here `\Web\InterpneuCore\Price\Persistence\Repository\IpExternalStockPriceRepository::load`.
