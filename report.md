# Introduction/Bussiness Problem
A client wants to open a restaurant in Toronto. In which neighborhood should he go for?
On one hand, well established restaurants are in a good location because they are well established and
get lots of clients. This might be because of other nearby venues or workplaces. On the other hand, 
competition might be an issue and not enough people will change restaurants. By utilizing geospatial data, 
we will try to find neighborhoods similar to restaurant packed ones and try to find the ones with the least
amount of restaurants.

# Data
## Data Sources
I am using wikipedia for a list of Toronto neighborhoods by postal codes. https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M
Some previous csv file, https://cocl.us/Geospatial_data, from coursera class with the latitude and longitude coordinates 
of the  neighborhoods is also used. FourSquare API is used to explore 
venues of the neighborhoods by utilizing previous coordinates.

By querying the FourSquare API with coordinates of neighborhoods, one gets nearby venues including restaurants.
With the assumption that other venues drive interest in restaurants, I will analyze what neighborhoods with
a high number of restaurants have in common.

The FourSquare API endpoint i'll use is the explore one. https://api.foursquare.com/v2/venues/explore
By giving a coordinate and a radius, it returns a list of nearby venues in json format with its name, location in
latitude/longitude and category. It has extra information like photos, but I will not need those. For example,
at lat 43.753259 and long -79.329656 with a radius of 500 meters, explore endpoint returns a json that looks like:

{'meta': {'code': 200, 'requestId': '5fd520abe95789054963179c'}, 'response': {'warning'....
... 'venue': {'id': '4e8d9dcdd5fbbbb6b3003c7b', 'name': 'Brookbanks Park', 
'location': {'address': 'Toronto', 'lat': 43.751976046055574, 'lng': -79.33214044722958, ...
'categories': [{'id': '4bf58dd8d48988d163941735', 'name': 'Park', 'pluralName': 'Parks', ...
'venue': {'id': '4cb11e2075ebb60cd1c4caad', 'name': 'Variety Store', 
'location': {'address': '29 Valley Woods Road', 'lat': 43.75197441585782, ... 
'categories': [{'id': '4bf58dd8d48988d1f9941735', 'name': 'Food & Drink Shop', 'pluralName': 'Food & Drink Shops',... 

By parsing the json I can get venues Brookbanks Park and Variety Store.

## Data Transformation
The categories are extracted and every category with restaurant in the name, for example, 'Italian Restaurant' 
or 'African Restaurant', is changed to 'Restaurant'. Every venue category is one hot encoded and summed 
per neighborhood giving a count of how many venues of that type are there. One can see that the neighborhood 
*Commerce Court* has 31 restaurants, being pretty packed.

#Methodology
##Exploratory Analysis
I want to test if some venues drive the demand for restaurants in the neighborhood up. For example,
if I scatterplot the restaurant category vs train station category, I get that only 4 neighborhood groups
have train station and 3 of them have more than 15 restaurants. The neighborhood of Kennedy Park, Ionview and
East Birchmount Park has 0 restaurants and one train station. This might be a good spot for a new restaurant.
If I scatterplot restaurant category vs hotel category, I also get that neighborhoods with hotels have more
restaurants. This suggests Richmond, Adelaide and King might be good spots to open a new restaurant.

I can linear regress every category over the restaurant category, but there are 223 columns for 93 rows when 
venue category is onehot encoded. This means a good fit is obtained with score 0.999996613012026, but the coeficients are 
hard to interpret. One can fit the top 25, by number of restaurants, and use the model to predict the rest of the 
neighborhoods. With that done and checked for a predicted number of restaurants higher than actual might indicate a good
spot.

##Clustering
If I remove the restaurant count and cluster the neighborhoods by the other counts, I expect to find neighborhoods that 
are similar to high restaurants' one but not much restaurants. Those neighborhoods are the ones with potential for new 
restaurants. The counts will be scaled so that each venue category has a balanced contribution to the distance measures.

I sort the neighborhoods in descending order of number of restaurants and look at the assigned clusters. Berczy Park is
in the same cluster of neighboorhoods with 20+ restaurants, but it has only 13.

#Results and Conclusion
After drawing a map and putting a dot on each neighborhood coordinate, I see that the cluster of neighborhoods packed with 
restaurants is also spatial. They are all near Commerce Court and Toronto Union Station. So k-means is selecting this area
for a cluster.

By using this cluster to open a new restaurant, there is no much avoidance of competition. A new restaurant in Berczy Park is
competing with the area, but the area is a high population traffic one, with people coming and going to the station.
