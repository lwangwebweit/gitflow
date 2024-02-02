# stock

#### stock import

full import daily at 3:20

```
20 3 * * * php -dmemory_limit=-1 /var/www/share/interstore.pneu.com/current/bin/console interpneu:import:stocks > /dev/null # full stock import daily at 3:20
```

delta update every 5 min

```
*/5 * * * * php -dmemory_limit=-1 /var/www/share/interstore.pneu.com/current/bin/console interpneu:update:stocks > /dev/null # stock update at every 5th minute
```

#### external stock import

full import daily at 3:35

```
35 3 * * * php -dmemory_limit=-1 /var/www/share/interstore.pneu.com/current/bin/console interpneu:import:external-stocks # external stock import full daily at 3:35
```

delta update every 5 minutes

```
*/5 * * * * php -dmemory_limit=-1 /var/www/share/interstore.pneu.com/current/bin/console interpneu:update:external-stocks > /dev/null # update external stock at every 5th minute
```

#### customer stock import

import every 1 hour&#x20;

```
0 * * * * php -dmemory_limit=-1 /var/www/share/interstore.pneu.com/current/bin/console interpneu:import:customer-stock > /dev/null # customer stock import every 1 hour
```

#### stock import
