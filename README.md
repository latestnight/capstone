# capstone

Google API query steps:

All queries based on information learned from https://towardsdatascience.com/how-to-use-the-google-places-api-for-location-analysis-and-more-17e48f8f25b1

Google Places type values that are under potential consideration for 'food' resources:

Table 1: Place types

Food/drink/meal related

Bakery
bar
cafe
gas_station
liquor_store
meal_delivery
meal_takeaway
restaurant
supermarket


# Ideas for query plan, in no particular order:

1. Google Places seems to have limit of 60 results per query. In order to maximize data harvesting, run individual queries for each zipcode in Nashville.
    From article linked above: "Since I was looping through several zip codes to do searches, I then appended that data frame to a master data frame and constructed a new column to filter out duplicates to only keep unique values.""
