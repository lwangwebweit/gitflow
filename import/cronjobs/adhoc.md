# adhoc

#### adhocAccount import daily every 15min

15 \* \* \* \* php -dmemory\_limit=-1 /var/www/share/interstore.pneu.com/current/bin/console interpneu:import:adhocaccount > /dev/null # delta adhocaccount import at minute 15. every hour

#### adhocmanufacturerbrand once a day at 3:50

50 3 \* \* \* php -dmemory\_limit=-1 /var/www/share/interstore.pneu.com/current/bin/console interpneu:import:adhocmanufacturerbrand > /dev/null # full import adhocmanufacturerbrand daily at 3:50

#### adhocmanufactureripcustomer daily every 15min

15 \* \* \* \* php -dmemory\_limit=-1 /var/www/share/interstore.pneu.com/current/bin/console interpneu:import:adhocmanufactureripcustomer > /dev/null # delta import adhocmanufactureripcustomer at minute 15. every hour
