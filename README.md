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

Jupyter slides??? (in place of powerpoint)
Rise (jupyter extension; html that can display folium maps etc)




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

# 22 count, official USPS Nashville Zip codes (that are not specially assigned to a company/organization or PO Box)
37201
37203
37204
37205
37206
37207
37208
37209
37210
37211
37212
37213
37214
37215
37216
37217
37218
37219
37220
37221
37228
37238

# 25 count (including double 37207) Nashville Zip codes extracted from GeoJSON sourced directly from data.nashville.gov

37221
37214
37204
37232
37205
37217
37213
37246
37228
37210
37209
37207
37240
37206
37212
37216
37219
37207
37203
37211
37218
37208
37220
37215
37201



# overall data gathering/cleaning steps:
  1. Google Places api
  2.
  3. Clean up individual grocery store CSVs in Excel to delete 'kroger pharmacy' types quickly
    a. leaving within CSVs 'honorable mentions', stores that weren't intentionally captured but are unique
  4. drop duplicate addresses in Pandas
