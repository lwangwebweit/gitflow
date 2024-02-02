# other imports

#### import delivery times every minute

php -dmemory\_limit=-1 /var/www/share/interstore.pneu.com/current/bin/console interpneu:import:deliverytimes > /dev/null&#x20;

#### import tracking information every minute

php -dmemory\_limit=-1 /var/www/share/interstore.pneu.com/current/bin/console web:order:trackingimport > /dev/null&#x20;

#### import order status every minute

php -dmemory\_limit=-1 /var/www/share/interstore.pneu.com/current/bin/console web:order:statusimport > /dev/null&#x20;

#### import currency  daily at 2:45

bash /var/www/share/interstore.pneu.com/current/custom/static-plugins/WebInterpneuCore/bin/import-currency-rates.sh -s=/var/www/share/sharedmedia/import/sap/currency -d=/var/www/share/import > /dev/null&#x20;

#### import carrier at minute 30. every hour

php -dmemory\_limit=-1 /var/www/share/interstore.pneu.com/current/bin/console interpneu:import:carrier > /dev/null&#x20;

