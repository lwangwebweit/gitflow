# AdHoc Accounts

**Command:**&#x20;

```
interpneu:import:adhocaccount
```

Default filename: adhoc\_account.csv

Optional parameter filename is supported

Class: `\Web\InterpneuB2B\Import\Communication\Commands\Sap\ImportAdhocAccount`



**Importer:**&#x20;

Class: `\Web\InterpneuB2B\Import\Business\Importer\Sap\AdhocAccountImporter`



**CSV Structure:**

Example row:

```
T|0130|R1P|BRIDGESTONE|http://adhoc.bridgestone.eu/test/adhoc|A2R5|XML|113930|ADHOC_E4T|abc|Christian Aberle|06172 - 408 276|Christian.ABERLE@bridgestone.eu|23.09.2022|10:02:00|ANDY||||BUYER=CONSIGNEE|1
```

Explanation:

1. `T` - Environment, Test or Production&#x20;
2. `0130` - Customer Number
3. `R1P` - Client Name
4. `BRIDGESTONE` - Manufacturer
5. URL
6. `A2R5` - Api Version
7. `XML` - Not imported
8. `113930` - Account Number
9. `ADHOC_E4T` - Username (used for authentication)
10. `abc` - Password
11. `Christian Aberle` - Full Name
12. `06172 - 408 276` - Phone Number
13. `Christian.ABERLE@bridgestone.eu` - Email address
14. `23.09.2022` - Not imported
15. `10:02:00` - Not imported
16. `ANDY` - Not imported
17. Not imported
18. Not imported
19. Not imported
20. `BUYER=CONSIGNEE` - Buyer Consignee Mode (details in \Web\InterpneuB2B\AdHoc\Struct\BuyerConsigneeMode::fromCsvValue)
21. `1` - Active (send order to SAP and API Provider) or inactive (inquiry order in SAP only)



