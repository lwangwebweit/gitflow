---
description: Flat Tables
---

# Models / Entities (Table Structure)

### Database Structure

Each concrete offer is saved in `ip_offer` database table. This table basically saves the same data as `cart` table. So each offer is a saved offer cart. In `customer_id` is saved the `debtor_id` of the contact so all contacts of that debtor can see the offers. In `offer` is saved the serialised offer cart.

For each debtor the selected calculation type is set in database table `ip_offer_calculation` column `calculation_type`. Enum values are set in `\Web\InterpneuCore\EndCustomer\Calculation\Enum\OfferCalculationTypeEnum`.

If the contact has permissions to see prices that their client (end-customer) will see then there is a button "End Customer Advisor" that changes the prices of the product from their actual prices that Interpneu sells them for to new calculated prices for end-customer. These end-customer prices are calculated in three ways:

1. RRP (Recommended Retail Prices) - We import these data from Interpneu A927. Prices that have price list value starting with `E` are RRP prices. `\Web\InterpneuCore\Price\Business\Mapper\RecommendedRetailPriceMapper`. RRP prices are imported with VAT.
2. Easy Calculation - In database table `ip_offer_calculation` there are two columns named `simple_surcharge` (fixed amount surcharge) and `simple_surcharge_percent`. For some reason right now these are not used to set the simple surcharge values.
3. Complex Calculation - For different groups of products there are set different fixed surcharges and/or percentage surcharges. Products groups are defined by Interpneu and those are found in database table `ip_offer_calcualtion_product_group`. Then in database table `ip_offer_calcualtion_debtor_product_group` for each debtor and each production group the contact can set the fixed surcharge in column `surcharge` and percent surcharge in `surcharge_percent`. ( #BUG There is a bad implementation right now that surcharges for simple surcharges are not set in database table `ip_offer_calculation` in columns `simple_surcharge` and `simple_surcharge_percent` but instead in table `ip_offer_calcualtion_debtor_product_group` all values in columns `surcharge` and `surcharge_percent` are changed. The fix should be made here `\Web\InterpneuCore\Price\Business\CustomerPriceCalculator\Resolvers\SimplePriceCalculatorResolver` to get the data from database table `ip_offer_calculation` columns `simple_surcharge` and `simple_surcharge_percent`. Also change how the offer calculation setup is stored by extracting `SimpleOfferCalculation` from `\Web\InterpneuCore\EndCustomer\Calculation\Strategy\ComplexOfferCalculation` \`\`)

The formula to calculate simple and complex prices is `((debtor net price + X EUR) * %) + VAT`.

The URL to make these changes can be found at `/offercalculation` and controller `\Web\InterpneuB2B\Offer\Calculation\Storefront\Controller\OfferCalculationController`. Tip: B2B controllers have their route set in controller service definition in XML.
