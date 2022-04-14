# data resources
Google Places API
CDC PLACES collaboration, 500 Cities datasets: https://www.cdc.gov/places/
  2016 release: https://chronicdata.cdc.gov/500-Cities-Places/500-Cities-Census-Tract-level-Data-GIS-Friendly-Fo/5mtz-k78d
  2017 release: https://chronicdata.cdc.gov/500-Cities-Places/500-Cities-Census-Tract-level-Data-GIS-Friendly-Fo/kucs-wizg
  2018 release: https://chronicdata.cdc.gov/500-Cities-Places/500-Cities-Census-Tract-level-Data-GIS-Friendly-Fo/k25u-mg9b
  2019 release: https://chronicdata.cdc.gov/500-Cities-Places/500-Cities-Census-Tract-level-Data-GIS-Friendly-Fo/k86t-wghb

CDC PLACES collaboration, after the study condensed all Nashville data into one Nashville-Davidson county entry:
  2020 release: https://chronicdata.cdc.gov/500-Cities-Places/PLACES-Place-Data-GIS-Friendly-Format-2020-release/ndzg-9nmv
  2021 release: https://chronicdata.cdc.gov/500-Cities-Places/PLACES-Place-Data-GIS-Friendly-Format-2021-release/vgc8-iyc4




# capstone




Google API query steps:

All queries based on information learned from https://towardsdatascience.com/how-to-use-the-google-places-api-for-location-analysis-and-more-17e48f8f25b1

Google Places type values that are under potential consideration for 'food' resources:

Table 1: Place types

Food/drink/meal related

Bakery
food
bar *
cafe
gas_station
convenience_store
liquor_store *
meal_delivery
meal_takeaway
restaurant
supermarket
grocery_or_supermarket ?


# Ideas for query plan, in no particular order:

1. Google Places seems to have limit of 60 results per query. In order to maximize data harvesting, run individual queries for each zipcode in Nashville.
    From article linked above: "Since I was looping through several zip codes to do searches, I then appended that data frame to a master data frame and constructed a new column to filter out duplicates to only keep unique values.""

2. import zipcodes.geojson from other projects, to obtain Nashville zipcodes after filtering geodataframe with .isin()

3. Places API does not seem to be capable of returning search results containing only a specific zipcode. I am now thinking that I may have to do a query with the `location=` parameter set to the lat/lng coordinates of each zipcode's centroid. After performing every query, I will then have to filter out any duplicate results.
