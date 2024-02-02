---
description: >-
  domain driven design is implemented by defining the whole offer management
  part as a Bundle and link the bundle to the b2b plugin
---

# Domain driven design

add Bundle definition to the OfferManagement folder

![](<../.gitbook/assets/Screenshot 2023-07-24 at 07.48.43.png>)

link Bundle to the B2b plugin, and it will be picked up in the shopware plugin lifecycle

```php
// in the b2b plugin definition WebInterpneuB2B.php
public function getAdditionalBundles(AdditionalBundleParameters $parameters): array
{
    return array_merge(
        parent::getAdditionalBundles($parameters),
        [new OfferManagementBundle()],
    );
}
```

backend code structure is defined in its own `DependencyInjection` setting&#x20;

frontend code just work the shopware standard way under the Bundle folder
