# Search overview

When making a request to load a page that is created in shopware admin at /Catalogues/Category then the route that takes care of handeling that page is `\Shopware\Storefront\Controller\NavigationController::index`.

Then everything is loaded in `\Shopware\Storefront\Page\Navigation\NavigationPageLoader::load`.

Navigation Id is the id of the category page and is stored in database table `Category`.

Each category is connected to a CMS page. The Cached CMS page is attempted to be loaded from `\Shopware\Core\Content\Category\SalesChannel\CachedCategoryRoute::load`- If there is no cache hit the the CMS is loaded in `\Web\InterpneuB2B\Core\Content\Category\SalesChannel\CategoryRouteDecorator::load`

If there is not search requests made the the page loads a different CMS page than the result page called `InitialCmsPage` if the request has a search parameter then the actual CMS page is loaded `\Shopware\Core\Content\Category\SalesChannel\CategoryRoute::load`.

The the CMS page is loaded at `\Shopware\Core\Content\Category\SalesChannel\CategoryRoute:101`

Each CMS page, has sections, which have block which have slots. All slots data is resolved at `\Shopware\Core\Content\Cms\SalesChannel\SalesChannelCmsPageLoader:94`

Then at `\Shopware\Core\Content\Cms\DataResolver\CmsSlotsDataResolver::resolve` for each CMS Slot that has a resolver `\Shopware\Core\Content\Cms\DataResolver\Element\AbstractCmsElementResolver` defined the methods `collect` then `enrich` are called.

In showing the listing of search the resolver `\Shopware\Core\Content\Product\Cms\ProductListingCmsElementResolver` is the point where all the filtering and elastic-search requests are made by calling the listingRoute `\Shopware\Core\Content\Product\SalesChannel\Listing\ResolveCriteriaProductListingRoute::load` at `\Shopware\Core\Content\Product\Cms\ProductListingCmsElementResolver:61` .

When the event `\Shopware\Core\Content\Product\Events\ProductListingCriteriaEvent` is dispatched, the current subscribers to this event are: 1. `\Web\InterpneuCore\Product\Business\Subscriber\ManufacurerPromotionsSubscriber::onCriteriaListing` 2. `\Web\InterpneuCore\Product\Business\Subscriber\TestReportsSubscriber::onCriteriaListing` 3. `\Web\InterpneuCore\Product\Business\Subscriber\PropertySubscriber::onCriteriaListing` 4. `\Web\InterpneuCore\Search\Communication\EventSubscriber\ProductListingCriteriaEventSubscriber::onProductListingCriteriaBuild` //TODO: Go into detail for this 5. `\Web\VideoPlus\Subscriber\Filter\VideoFilterSubscriber::handleListingRequest`

Then if there is a cached result for the current criteria that was formed in the previous event then it is returned at `\Shopware\Core\Content\Product\SalesChannel\Listing\CachedProductListingRoute::load` if not then a new result is queried at `\Shopware\Core\Content\Product\SalesChannel\Listing\ProductListingRoute::load`.

Which loads the results at `\Web\InterpneuB2B\Search\Business\Loader\ProductListingLoaderDecorator::load` and //TODO: There is more stuff going on `\Web\InterpneuCore\Search\Communication\Elasticsearch\Definition\IpElasticsearchProductDefinition`. Check how this forms the Elasticsearch Query

#### ProductListingCriteriaEventSubscriber::onProductListingCriteriaBuild

The main point where are criteria builders are iterated is at `\Web\InterpneuCore\Search\Business\CriteriaHandler\FilterCriteriaHandler::handle`. All Criteria Builders in their xml files have the tag `interpneu.filter.criteria_builder`.

I am taking the example of an `interpneu.filter.criteria_builder` `\Web\InterpneuB2B\Search\Business\CriteriaBuilder\SearchType\TireMatchCodeCriteriaBuilder`. Match code and parameter criteria builders for tires, rims and others work similarly.

First we need to make sure that the matchcode writtein is valid, and that is done at `\Web\InterpneuB2B\Search\Business\CriteriaBuilder\SearchType\TireMatchCodeCriteriaBuilder:60` through `\Web\InterpneuCore\Search\Business\MatchCode\Validator\TireMatchCodeValidator`.
