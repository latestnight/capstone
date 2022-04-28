Table of Contents

    Motivation
    Data Sources
    Technologies
    Challenges and Limitations
        API query

    Link to Dashboard
    testing

# Motivation
There are a few reasons for why I chose my capstone topic:
  1. I have an interest in public health. I have worked in healthcare for the past seven years, and it is well understood how dietary choices have a great impact on one's health.
  2. I love food. We can all agree that food is on the short list of 'things' that are loved by every single human on this planet. The availability of delicious food is a concern for everybody. The availability of healthy and nutricious food may be less of a concern for some, yet it is an even more important matter.
  3. The Piggly Wiggly on West End closed up shop after years of providing fresh groceries to local residents. I happen to live very close to this Piggly Wiggly. Now keep in mind I never really shopped there much at all - but that doesn't mean other people didn't. The closure of a large number of people's only local grocery store meant that purchasing fresh food became a lot more difficult and involved. One of the concepts encompassing this problem is the idea of the food desert.
  4. Food deserts are something that I have been familiar with for a little while now, but I still didn't know a whole lot. As I previously mentioned, I never really shop at the now shuttered Piggly Wiggly. I choose to drive down West End to either the Kroger, Publix, and occasionally Trader Joe's. These grocery stores are around 2.5 miles from my house, and it isn't the biggest hassle driving to them. Not everyone has use of a car like I do, for one reason or another.

# What is a Food Desert?
The exact definition of what a food desert is seems to still be developing, based on some of the material I read. One particularly important characteristic is that in the context of distance traveled/commuted, any distance to a grocery store that is greater than one mile in an urban setting will place someone in a food desert. This characteristic changes when looking at rural communities, where the distance is increased to ten miles. Some academic studies include household income levels as one of the metrics observed. There is also the decision about if whether or not convenience stores, corner markets, and restaurants should be considered when looking at food sources. Another important dimension is the presence or lack of transportation, whether it's a personally owned vehicle, or public transportation.

# Public Health Implications
As mentioned above, it is already well understood that dietary choices will have an impact on someone's health. Dealing with chronic disease is a daily task for many members of our local population. My analysis focuses on chronic diseases where diet is thought to be a notable component of disease onset or progression. These diseases are the following: hypertension, heart disease, diabetes mellitus, hyperlipidemia/high cholesterol, obesity, and stroke.


# Data Sources
Google Places API
U.S. Centers for Disease Control and Prevention PLACES collaboration, 500 Cities dataset:
                  2019 release: https://chronicdata.cdc.gov/500-Cities-Places/500-Cities-Census-Tract-level-Data-GIS-Friendly-Fo/k86t-wghb
U.S. Department of Housing and Urban Development Office of Policy Development and Research: https://www.huduser.gov/portal/datasets/usps_crosswalk.html


# Technologies
Python
Jupyter Notebook
requests
pandas/GeoPandas
*NumPy
*matplotlib.pyplot
Seaborn
folium
Tableau
Excel
PowerPoint




# Data Collection and Analysis Process
1. Imported geojson containing zip code, city name, and zip code geometry.
    a. Filtered geojson to only contain zip codes within Nashville city limits. My analysis is not looking at the entire Metro Nashville-Davidson County area.
    b. 37207 is listed twice. Determined that this double listing was intentional, as 37207 has a geographically 'separate' polygon when looking at the map geometry. Based on subsequent query technique, decided it was safe to remove smaller polygon from geojson.
2. After creating list of Nashville zip codes, I created a centroid for each zip code polygon.
3. Next I created a list of the latitude/longitude for each centroid. The lat/long will be utilized in my API query.
4. Imported geojson into folium to create base zip code map.
5. Now that I have a list of the zip codes I will be searching within, I began experimenting with performing Google Places API queries.
    a. Requests utilized to call API
    b. Time package utilized to prevent query timeout
6. Dictionary made for zip code centroid coordinates.
7. Dictionary made for approximate radius of search area
    a. API limitations made me decide to query by each zip code to ensure the most complete results.
    b. Duplicates with API query will be a given. To reduce the amount of duplicates, and to ensure that query is focused enough on specific zip code: MeasureControl folium plugin utilized to make approximate measurement of zip code at widest point. Halved measurements placed into radius dictionary.
8. Both zip code radius and centroid coordinates combined into master dictionary, to aid in subsequent queries.
9. Manually performing trial and error query takes far too long - time to automate queries by creating a query function.
    a. Query function takes three arguments: zip code to be queried, centroid coordinates, and zip code radius.
10. Query function modified for different establishment searches: grocery stores, gas stations, convenience stores, and restaurants. Each function returns a DataFrame that is created from a dictionary. A .csv is automatically created to save query results locally.
11. Cleaning my newly acquired data is revealing that some aspects of the cleaning may be far too subjective. Decision made to limit scope of analysis to only cover established large chain grocery stores.
12. Query function modified/created for all major grocery stores/supermarkets with local presence.
    Each query returns the following:
      a. Grocery Store name
      b. Formatted address
      c. Store lat/long coordinates
      d. Places API 'type' categories
13. For-loop written to automate the performing of all grocery store queries.
14. Code written to append all query results into a master DataFrame.
15. Utilized grocery store DataFrame to load all points into folium
    a. Circle markers utilized in folium
    b. Radius of circle marker set to 1609 meters to provide coverage of one mile radius from all grocery store locations
    c. Radius of one mile from all markers shows area on map that does not exist within a potential food desert.
    d. Uncovered 'negative space' on map shows geographic areas that are potential food deserts.
16. CDC data cleaned to only contain data for Nashville.
17. Original intent was to show disease prevalence over time. Four years of disease prevalence measures collected for all Nashville census tracts. Two additional years available from CDC that contain same metrics, but are condensed for Nashville area as a whole. API query did not collect data on open/closed status of grocery stores, so the lack of a grocery store presence timeline made me decided to only focus on most recent CDC data that covered all census tracts.
18. Original CDC data has census tract number, as well as lat/long coordinates for each tract. Decision made to join all data on zip codes.
19. HUD crosswalk file obtained to cross reference census tracts and determine what tracts are in which zip codes.
    a. Due to how census is performed, some tracts border two or more zip codes.
    b. Zip code category 'unstacked' so each CSV entry only has one row.
20. Zip code and CDC geodata loaded into Tableau
21. Food desert map recreated in Tableau. Calculated field created to recreate grocery store coverage marks.
22. Disease data by zip code added as Tableau map layer. Disease prevalence displayed as average of averages.
23. Python utilized for EDA. Value counts performed on studied zip codes to see count of total grocery stores per zip code.
24. Created DataFrame of disease prevalence and zip codes. Zip codes grouped according to number of grocery stores within zip, and then disease prevalence averaged.
25. Scatter plots created within Tableau to visually examine if there seems to be any relationship between number of available grocery stores and the prevalence of disease in a particular zip code.

# Challenges and Limitations
  It was really fun learning how to use Google's API to perform queries. One thing I quickly learned however is that the Places API has some nuances that limit how the queries are exactly performed. The API will only return 60 results per query. This can be problematic for searches performed over a large area. Places API will also use an internal algorithm to choose what is displayed in the search results. The smaller your geographic search area, the more accurate and fine tuned the results will be. This poses a problem when looking at larger areas such as an entire zip code. This is why I decided to take an approximate measurement of each zip code's size. In the API parameters, I utilized the zip code centroid coordinates to specify where I wanted to cast my query 'net'. The radius parameter for the API was then utilized to ensure that the 'net' I casted was only large enough to cover the approximate size of the zip code. This helped lessen the amount of duplicate search results, while also fine tuning the relevance of the results.

  The decision to only focus on large chain grocery stores was made as I was manually cleaning my generated CSVs. While going down the list, I began to manually recategorize stores as needed. Many smaller markets, corner stores and other convenience stores were included in the grocery store specific query. While a lot of these stores may sell some healthier food choices, it was not a guarantee. Long story short, I felt that there was too much subjectivity in casting a judgement on whether or not a store should be included in my grocery store list. Choosing to only query for established grocery store chains acted as a control of sorts to only look at establishments that can provide a community with fresh produce and meats, in addition to all the beloved processed food that people enjoy. The availability of healthy food was the primary concern.


#Dashboard Explanation
The biggest visualization on my dashboard is the food desert map. This interactive map displays all grocery stores and their geographic reach, overlayed on top of Nashville's zip code boundaries. The zip codes are colored based upon average disease prevalence. There are two drop down menus that allow you to customize the map. One drop down menu lets the user change which chronic disease data is displayed. The grocery store drop down menu lets the user pick and choose which grocery stores are displayed. A user can use this menu to simulate a grocery store closing, or a chain going out of business.

On the bottom of the dashboard is a bar graph that displays the total number of grocery stores per zip code. The visualization on the right of the dashboard shows the total number of distinct store businesses that have a presence in a particular zip code. Both of these visualizations can help the user understand what zip codes are most vulnerable if a business were to be shuttered.

# Link to Dashboard
