# Pricing Calculation

<details>

<summary>List of subscribers  to "controller.event" which is dispatched when a controller is resolved for the given request</summary>



1. "Web\VideoPlus\Listener\VideoDeleteListener::onKernelController"&#x20;
2. "Symfony\Bundle\FrameworkBundle\DataCollector\RouterDataCollector::onKernelController"
3. "Symfony\Component\HttpKernel\DataCollector\RequestDataCollector::onKernelController"
4. "Sensio\Bundle\FrameworkExtraBundle\EventListener\ControllerListener::onKernelController"
5. "Sensio\Bundle\FrameworkExtraBundle\EventListener\ParamConverterListener::onKernelController"
6. "Sensio\Bundle\FrameworkExtraBundle\EventListener\HttpCacheListener::onKernelController"
7. "SwagB2bPlatform\Routing\RouteScopeResolver::resolveScope"
8. **"Shopware\Core\Framework\Api\EventListener\Authentication\SalesChannelAuthenticationListener::validateRequest"**
9. "Shopware\Core\Framework\Api\EventListener\Authentication\ApiAuthenticationListener::validateRequest"
10. "Shopware\Storefront\Framework\Csrf\CsrfRouteListener::csrfCheck"
11. "Shopware\Core\Framework\Routing\ContextResolverListener::resolveContext"
12. &#x20;"Web\InterpneuB2B\Customer\Subscriber\ControllerSubscriber::onKernelController"&#x20;
13. "Shopware\Core\Framework\Routing\RouteScopeListener::checkScope"
14. "Shopware\Core\Framework\Api\Acl\AclAnnotationValidator::validate"
15. "Shopware\Storefront\Framework\Captcha\CaptchaRouteListener::validateCaptcha"&#x20;
16. "Shopware\B2B\SalesRepresentative\BridgePlatform\SalesRepresentativeSubscriber::redirectSalesRepresentative"
17. "Web\InterpneuCore\Core\Framework\Routing\StorefrontSubscriberDecorator::preventPageLoadingFromXmlHttpRequest"
18. "Shopware\Core\Framework\Adapter\Cache\CacheStateSubscriber::setStates" 18 = "Shopware\Core\Framework\Api\EventListener\ExpectationSubscriber::checkExpectations"
19. "Shopware\Storefront\Framework\AffiliateTracking\AffiliateTrackingListener::checkAffiliateTracking"
20. "SwagB2bPlatform\Subscriber\FrontendAccountFirewall::redirectToController"
21. "SwagB2bPlatform\Subscriber\B2bModuleFirewall::redirectOutOfB2b"
22. "Shopware\B2B\AclRoute\BridgePlatform\RequestInterceptorSubscriber::redirectIfInaccessible"
23. "NetInventors\NetiNextAdminTools\Subscriber\ApiSubscriber::onKernelController"
24. "Sensio\Bundle\FrameworkExtraBundle\EventListener\TemplateListener::onKernelController"

</details>

The subscriber number is important in pricing calculation because `\Web\InterpneuB2B\Core\System\SalesChannelContextFactoryDecorator::addOfferCalculationExtension` the OfferCalculationModel is set to context which will be used in calculation.&#x20;



For example when Product listing is called `\Shopware\Core\Content\Product\SalesChannel\Listing\ProductListingLoader::load` , the products are load. Everything starts here, when the products are actually fetched from database in `\Shopware\Core\System\SalesChannel\Entity\SalesChannelRepository::read`. Then the calculator is called to calculate the prices of load products in `\Web\InterpneuCore\Price\Business\PriceCalculator\IpProductPriceCalculator::calculate` .&#x20;

Then the subscriber `\Web\InterpneuB2B\OfferManagement\OfferPriceCalculation\Subscriber\IpProductPriceCalculatorSubscriber::endUserProductsPriceCalculator` is called to go to `\Web\InterpneuB2B\OfferManagement\OfferPriceCalculation\Business\EndUserViewPriceCalculator::calculateList` to calculate the prices of the loaded products.&#x20;

Then in `\Web\InterpneuB2B\OfferManagement\OfferPriceCalculation\CustomerPrice\CustomerPriceCalculator::getPrices` for each product that was loaded the prices are calculated in `\Web\InterpneuB2B\OfferManagement\OfferPriceCalculation\CustomerPrice\Resolvers\BasePriceCalculatorResolver::getPrice`

In `\Web\InterpneuB2B\OfferManagement\OfferPriceCalculation\CustomerPrice\Resolvers\PriceCalculatorHelper::findProductGroupCalculationElement` there are three possible results based on the `CalculationModel`&#x20;

1. If the `CalculationModel` is `RRP` then this method return null.
2. If the `CalculationModel` is `simple` then there is only one OfferCalculationDebtorProductGroupModel meaning all products will get the same percentage and fixed surcharge added to the EndUserPrice.&#x20;
3. If the CalculationModel is complex then for the product there is a rulechecker implemented to determined if the current product that the price is being calculated belongs to any of the categories in complex CalculationModel. If that is the case then the right `OfferCalculationDebtorProductGroupModel` will be fetched to calculate price based on  fixed surcharge added to the EndUserPrice of that `OfferCalculationDebtorProductGroupModel`

Then if the `PriceOptionExtension` is set to the product we calculate the EndUserPrices for each of the three possible stocks the product can be in: own, external and adhoc.&#x20;

