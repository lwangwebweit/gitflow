# Warehouses

### Warehouse type

Products that are sold in our system come from three different types of warehouse:

1. Internal Warehouse Type AKA Interpneu Owned Warehouse or Interpneu Warehouse or Warehouse 1 - Interpneu owns four different warehouses located in different cities in Germany. If a product is one of these Interpneu owned warehouses, it can be allocated to only ONE warehouse. Each warehouse of Interpneu owned warehouse stores their own specific types of products. Products that are stored here always have the highest priority to be sold. The stock is saved in database table `product` column `stock`. Whereas the prices are saved in database table `ip_price`.
   1. In city Speyer encode as warehouse 1000
   2. In city Karlsruhe encoded as warehouse 2000
   3. In city Nossen encoded as warehouse 3000
   4. In city Hainichen encoded as warehouse 4000.
2. External Warehouse Type AKA Interpneu Lager 2 or Warehouse 2 or Third party Warehouse or FLPM Warehouse - the stock and prices are saved in database table `ip_stocks`. Current external warehouses we have which are imported in defined intervals from Interpneu are:
   1. 819
   2. 822
   3. 1822
   4. 821
3. Industry Warehouse Type AKA AdHoc Warehouses - Prices for these products are saved in table `ip_price` but the stock is not saved in our system. The stock is got through API calls in different systems stored in database table `ip_adhoc_account`. Every time we show a product in listing or product detail page, or before the order is finalised we make a call to show the contact if the product has stock or not in Industry Warehouses. Based on the manufacturer of the product and sales channel the right API to call is chosen. Current manufacturer APIs we have which are imported in defined intervals from Interpneu are:
   1. BRIDGESTONE
   2. CONTINENTAL
   3. FALKEN
   4. GOODYEAR
   5. MICHELIN
   6. NOKIAN
   7. PIRELLI
   8. VREDESTEIN

### Comparison 3 warehouses

<table data-full-width="true"><thead><tr><th width="154.25"></th><th>Internal Warehouse</th><th>External Warehouse</th><th>Industry Warehouse (adhoc)</th></tr></thead><tbody><tr><td>stock condition</td><td>product.stock > 0</td><td>product.stock &#x3C;= 0 &#x26;&#x26; ip_stock > 0</td><td>customerId in ip_manufacturer_ip_customer</td></tr><tr><td>stock value</td><td>product.stock</td><td>ip_stocks</td><td>via api</td></tr><tr><td>saleschannel</td><td>all</td><td>all</td><td>reifen+, auto plus</td></tr><tr><td>warehouse id</td><td>product.customFields.own_stock_id</td><td>customer.customFields.web_external_stock_ids</td><td>ip_manufacturer_ip_customer.manufacturer</td></tr><tr><td>price from</td><td>ip_price</td><td>ip_stocks</td><td>ip_price</td></tr></tbody></table>

### Warehouse Inventory display

**At Interpneu & EROL**&#x20;

Only warehouse 1 is always displayed.\
→ If Warehouse 1 runs out of inventory, Warehouse 2 will be displayed at this location (only if there is available inventory for Warehouse 2).

**At Reifen1+, Carat, AutoPlus**

1. If the item is only available internally at warehouse 1 (no industry)\
   \= same logic as described above for IP & EROL
2. If this article is also available in the industry:
   1. Both warehouses (IP Speyer warehouse 1 and industrial warehouse) are always displayed at the same time if both have available inventory.
   2. If Warehouse 1 is no longer available but Industry still has inventory, Warehouse 1 will be grayed out and the industry inventory will continue to be displayed below it
   3. If **both** warehouses (IP Speyer Warehouse 1 & Industry) have no inventory, the external warehouse will step in. This then **visually replaces warehouse 1 at the top of the supplier selection** and is displayed (if available) with the corresponding stock.\
      → The industrial warehouse below remains grayed out. As soon as the industry or warehouse 1 has inventory again, the external warehouse disappears again according to this logic.

:bulb:In case that there is no prices in the system for Internal or External products, then there is not grayed out option in detail page. It only shows the Industrial option.&#x20;

### Useful SQLs

```sql
# fetch price from internal warehouse by product number and customer number
select ip.* from product p
join ip_price ip 
on p.product_number = ip.`product_number`
join customer c
on ip.price_list = JSON_EXTRACT(c.`custom_fields`, '$.pltyp') 
and ip.price_group = JSON_EXTRACT(c.`custom_fields`, '$.konda')
where p.product_number = '000000000000275121' and c.customer_number = '0694012124';
```

```sql
# find products with stock and price from warehouse 2
SELECT  
lower(hex(p.id)) as product_id,  
p.product_number as product_number,  
ip_stocks.stock as exteranl_stock,  
p.stock as internal_stock,  
ip_stocks.price as external_price,  
p.price->"$.cb7d2554b0ce847cd82f3ac9bd1c0dfca.net" as internal_price  
FROM  
ip_stocks  
JOIN product p on ip_stocks.product_number = p.product_number  
WHERE p.stock = 0 
AND ip_stocks.stock > 0;
```

```sql
# find customers with adhoc access by manufacturer
select c.customer_number from `ip_manufacturer_ip_customer` imic
join customer c
on imic.customer_id = c.id
where imic.manufacturer = 'CONTINENTAL';
```
