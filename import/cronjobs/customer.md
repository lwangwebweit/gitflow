# customer

#### full import runs once a day at 2:25 am

25 2 \* \* \* run-one bash /var/www/share/interstore.pneu.com/current/custom/static-plugins/WebInterpneuB2B/bin/import-debtors.sh /var/www/share/sharedmedia/import/sap/customer/kunden.csv.gz full > /dev/null # customer full import every day at 2:25

#### delta every 15 minute past every hour from 3-23 and at 0:15 and 1:15

\*/15 3-23,0,1 \* \* \* run-one bash /var/www/share/interstore.pneu.com/current/custom/static-plugins/WebInterpneuB2B/bin/import-debtors.sh /var/www/share/sharedmedia/import/sap/customer/kunden\_tag.csv.gz > /dev/null # import customers delta every 15 minute past every hour from 3-23 and at 0:15 and 1:15

following entities will be imported and updated

* customer
* debtor flag (b2b\_customer\_data)
* contact (b2b\_debtor\_contact)
* billing address
* shipping address



**Updated customer import (See DEVINTER-1714):**

The import processed is now split up. Extracting the archived SAP Files from `/var/www/share/sharedmedia/import/sap/customer` is handled by the basic `interpneu:import:extract:gzip:data` command while the import is done through the `interpneu:import:customers` command.\
It will write all data in batches of 100 as `\Web\InterpneuB2B\Import\Business\Importer\Sap\ImportCustomerMessage` to the default queue. `\Web\InterpneuB2B\Import\Business\Importer\Sap\ImportCustomerHandler` handles these messages and does the final import.\
For full imports the data is cleaned up by the `interpneu:import:delete-customers` command. It runs 1 hour later, to make sure that enough time has passed for all queue messages to be processed.
