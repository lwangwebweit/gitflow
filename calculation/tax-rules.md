---
description: tax rule defined in interpneu
---

# Tax rules

tax will be applied when

* shipping method is pick up
* or shipping country is DE or CH or the same as sales channel country (like france in Erol)
* or no customer or debtor (some error happens and fallback to with tax)

tax free when

* debtor country (customFields\['country']) is not in EU
* or shipping location country ($context->getShippingLocation()->getCountry()) is not in EU

if not matched before the tax status is determined by the vatId (customFields\['stceg'], not shopware standard)

* with vatId -> tax free
* with no vatId -> with tax
