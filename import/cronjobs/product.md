# product

#### product import file by file every minute

until php -dmemory.limit=-1 /var/www/share/interstore.pneu.com/current/bin/console interpneu:import:products --limitFiles=1 > /dev/null; do sleep 1; done&#x20;

product related information will be imported

* product data
* category
* deliveryTime
* download
* image
* manufacture
* manufacturePromotion
* productGroup
* property
* video
* ranking
* tyreLabel
* writelist
