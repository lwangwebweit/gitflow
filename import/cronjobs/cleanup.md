---
description: cleanup scripts running on the prod server
---

# cleanup

only files from last 7 days are kept in the following folders

* /var/www/share/sharedmedia/import/sap/adhoc # cleanup adhoc imported files
* /var/www/share/sharedmedia/import/acl/prod # cleanup acl folder
* /var/www/share/sharedmedia/import/sap/flpm # cleanup flpm files
* /var/www/share/interstore.pneu.com/shared/importDone # cleanup folders older than 7 days
* /var/www/share/interstore.pneu.com/shared/importDone # cleanup imports older than 7 days
* /var/www/share/sharedmedia/import/sap/stock # cleanup sap stock folder

following folders every x days

* 0 1 _/5 \* \* rm /var/www/share/import/prices/done/_ # delete price done folder every 5th day of month at 1

index update everyday&#x20;

0 7 \* \* \* php -dmemory\_limit=-1 /var/www/share/interstore.pneu.com/current/bin/console dal:refresh:index --use-queue > /dev/null # Indexes (Updates the indexes such as the category and product indexes and the SEO URLs) every day at 7 into queue
