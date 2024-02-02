# Troubleshooting

## **Product not found**

check product first in the Database and ElasticSearch

* if target product missing in the database, gdpr-dump from the correct env server
* if not found in elasticSearch, reindex the ES

## Cache

default ttl for cache is 3600s and if cache hit, product will not be loaded from DB & ES, set `INTERPNEU_CACHE_EXPIRY_TIME=1` in the .env could prevent cache when debugging

## Get search request to Elasticsearch

* Trigger request
* Go to your Symfony debug bar for the page, tab Elasticsearch
* Identify request, copy it (optionally prettyprinted)
* If no request found, cache might need to be disabled (see above)

## Get index request to Elasticsearch

* Trigger indexing of single product:\
  `bin/console interpneu:debug:index 000000000000439743`
* Alternatively saving on the admin might also work
* Starts at `\Web\InterpneuCore\Command\DebugDalIndexCommand::execute`
* Request to Elasticsearch should happen at\
  `\Elasticsearch\Client::bulk`
* Request body can be intercepted as JSON

## Elasticsearch access via GUI

* Install for example Elasticvue, a browser plugin available for several browsers
* Send search (or any REST) requests to your local elasticsearch at localhost:9200
* Access to dev/stage/prod
  * SSH access to server required
  * Tunnel access to your localhost port 9201 or 9202 for example\
    `ssh -L 9201:es-master:9200 <prodservername>`\
    `ssh -L 9202:stage-elasticsearch:9200 <stageservername>`
  * Then configure localhost:9201/9202 on your Elasticvue

