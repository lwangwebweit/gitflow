# move & unzip files

files from following folders are moved to \`/var/www/share/interstore.pneu.com/shared/importFolder/\`

* /var/www/share/sharedmedia/import/sap/ordrsp/   # move order status files to shopware importfolder at every minute
* find /var/www/share/sharedmedia/import/sap/tracking/  # moving tracking information files every minute to shopware importfolder

extract data

* 20 \* \* \* \* php /var/www/share/interstore.pneu.com/current/bin/console interpneu:import:extract:data > /dev/null # unzip import data every 20 minutes
* 1,6,11,16,21,26,31,36,41,46,51,56 \* \* \* \* php -dmemory\_limit=-1 /var/www/share/interstore.pneu.com/current/bin/console interpneu:import:extract:gzip:data > /dev/null # unzip import files at minute 1, 6, 11, 16, 21, 26, 31, 36, 41, 46, 51, and 56. every hour
