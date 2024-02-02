# combine search

### before search

in `FilterCriteriaHandler::handle`,  all the custom Criteria are added to the search criteria, the criteria builder implements the `CriteriaBuilderInterface,` for combined search the corresponding builder is `TireMatchCodeCombinedCriteriaBuilder` , where request is separated and parsed as front and back tire, the related properties are

* tire type
* width
* height
* diameter
* load index
* speed index

### search

the actual search is taking place in `ProductListingLoader::load`&#x20;

### after search

in `SearchResultsGroupingService` search result will be grouped into front and back tire, following rules will be check and only qualified matches will be returned

* matchManufacturer
* matchProfile
* matchEmergencyRunIdentification
* matchSpeedIndex
* matchLoadIndex

if combined products found, they will be added to `CombinedSearchExtension` as `ProductGroupingStruct`
