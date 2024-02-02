---
description: Explain how we calculate the shipping cost
---

# Shipping Costs

IpDeliveryCalculator.php is the class responsible for calculate the shipping cost per product\
Here are some general information

* First we group product in the cart by warehouse so if we have 4 product in the cart 2 of them from **Speyer** which has warehouse\_id **1000** and the other 2 from **Nossen** which has warehouse\_id **2000** \
  so we map them to be like this in this function **generateStockTable** \
  and also we add the shipping method under the warehouse\_id so may be one of the product that is coming from **Speyer** will be picked up from warehouse and the other one will be delivered to another shipping address\
  so the array will look like this\
  \
  1000\
  &#x20;     pick-up\
  &#x20;           line-itme\_1 (3 quantuity)\
  &#x20;     delivery-address\
  &#x20;           line-item\_2 (1 quantity)\
  2000\
  &#x20;     delivery-address\
  &#x20;           line-item\_3 (1 quantity)\
  &#x20;           line-item\_4 (1 quantity)
* Also we need to take care of 1 important thing is \
  how many line-item quantity is persist in every warehouse\
  we have this rule that there is **minimum shipping cost** added to line-item if there is only 1 quantity will be delivered form this warehouse\
  So in the previous example we will not apply **minimum shipping cost**
* We need to take care also for extra surcharge that can be applied to **every** **line-item**  and this depend on the table **ip\_freight\_costs** where the sending\_country is the billing address country and the destination\_country is the shipping address country\
  and also we have product\_type if the product is truck we select value with product\_type **YNFZ** otherwise  **YREI**
* Another extra surcharge that can be applied to **every** **line-item** is depend on the shipping address Special area from table **ip\_freight\_costs\_special\_areas** where abbreviation column is the shipping address country and postal\_code for the shipping address postal\_code
* Handling with configurator product is a little different that handling normal product\
  configurator product can constitute of more than product\
  So we will have for example (Rim and Tyre) and we handle them as bundle item and when we calculate the shipping cost for the bundle item not for each sub-product in the bundle\
  and in this case we will have to differentiate between normal product and configurator product&#x20;
* At the end we sum all the warehouses shipping cost to have **Shipping costs** and this value is the one display in the Summary section in the page

Now we go to Code :)

```php
function calculate 
```

this is the entry point function that is called in every request \
and every thing is called inside this function\
the first function that is called here is&#x20;

```php
function generateStockTable
```

and this function as I explain the first point is prepare the cart line-items depend on the warehouse and shipping methods

the second function that all calculation for shipping cost is happened in is

```php
function getShippingCosts
```

we loop on every warehouse that is result from **generateStockTable** function and loop also on every shipping methods\
and check if the shipping cost is pick-up => so there will be no shipping cost for this line-item\


```php
if ($shipping === (string) ShippingMethodEnum::PICK_UP()) {
    continue;
}
```

then we need to determine the shipping country and zip code that we will use to calculate the extra shipping cost&#x20;

```php
if ($shipping === (string) ShippingMethodEnum::DELIVERY_ADDRESS()) {
    $country = $deliveryData['shipping_country'];
    $zip = $deliveryData['shipping_country_postal_code'];
} else {
    $country = $deliveryData['billing_country'];
    $zip = $deliveryData['billing_country_postal_code'];
}
```

after-that we need to get the list of the product\_ids form line-items to know if they are Truck or at least one product has ipProductData from this function **determineTruckAndIpData**

&#x20;Then we check for warehouse quantity to know if we are going to apply **MinQuantitySurcharge** or not

```php
if ($warehouseLineItemsQuantity === 1) {
    if ($ipProductData !== null && $isTruck) {
        $deliveryCosts[self::MIN_QUANTITY_SURCHARGE] = self::MIN_QUANTITY_SURCHARGE_VALUE;
        $shippingCostsPerWarehouse += self::MIN_QUANTITY_SURCHARGE_VALUE;
    } else {
        $deliveryCosts[self::MIN_QUANTITY_SURCHARGE] = self::MIN_QUANTITY_SURCHARGE_VALUE_CAR;
        $shippingCostsPerWarehouse += self::MIN_QUANTITY_SURCHARGE_VALUE_CAR;
    }

    $totalMinQuantitySurcharge += $deliveryCosts[self::MIN_QUANTITY_SURCHARGE];
}
```

afterwords we calculate the standard delivery costs for warehouse

```php
$standardDeliveryCosts = $warehouseLineItemsQuantity * $this->getIpFreightCosts($isTruck, $deliveryData['billing_country'], $country);
$deliveryCosts[self::STANDARD_DELIVERY_COSTS] = $standardDeliveryCosts;
$totalStandardDeliveryCosts += $deliveryCosts[self::STANDARD_DELIVERY_COSTS];
$shippingCostsPerWarehouse += $standardDeliveryCosts;
```

and special area costs for warehouse

```php
$standardDeliveryCosts = $warehouseLineItemsQuantity * $this->getIpFreightCosts($isTruck, $deliveryData['billing_country'], $country);
$deliveryCosts[self::STANDARD_DELIVERY_COSTS] = $standardDeliveryCosts;
$totalStandardDeliveryCosts += $deliveryCosts[self::STANDARD_DELIVERY_COSTS];
$shippingCostsPerWarehouse += $standardDeliveryCosts;
```

after we calculate standard delivery costs and special area costs for warehouse\
we need to set those values for each line-item adding extension **lineItemShippingCostExtension**\
that is used in the twig file to display the correct shipping cost for every lint item\


```php
foreach ($lineItems as $lineItem) {
    if ($warehouseLineItemsQuantity !== 1) {
        $standardShippingCostsPerItem = $standardDeliveryCosts / $warehouseLineItemsQuantity;
        $totalStandardDeliveryCosts = $lineItem->getQuantity() * $standardShippingCostsPerItem;

        $specialAreaShippingCostsPerItem = $specialAreaCosts / $warehouseLineItemsQuantity;
        $totalSpecialAreaCosts = $lineItem->getQuantity() * $specialAreaShippingCostsPerItem;
    }

    $lineItem->addExtension(LineItemShippingCostExtension::EXTENSION_NAME, new LineItemShippingCostExtension($totalMinQuantitySurcharge, $totalStandardDeliveryCosts, $totalSpecialAreaCosts));
}
```

and then we set MinQuantitySurcharge, StandardDeliveryCosts and SpecialAreaCosts for each warehouse

```php
$deliveryCostsPerWarehouse[self::MIN_QUANTITY_SURCHARGE] = $totalMinQuantitySurcharge;
$deliveryCostsPerWarehouse[self::STANDARD_DELIVERY_COSTS] = $totalStandardDeliveryCosts;
$deliveryCostsPerWarehouse[self::SPECIAL_AREA_COST] = $totalSpecialAreaCosts;
```

