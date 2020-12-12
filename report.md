# Introduction/Bussiness Problem
A client wants to open a restaurant in Toronto. In which neighborhood should he go for?
On one hand, well established restaurants are in a good location because they are well established and
get lots of clients. This might be because of other nearby venues or workplaces. On the other hand, 
competition might be an issue and not enough people will change restaurants. By utilizing geospatial data, 
we will try to find neighborhoods similar to restaurant packed ones and try to find the ones with the least
amount of restaurants.

# Data
We are using wikipedia for a list of Toronto neighborhoods by postal codes. https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M
Some previous csv file, https://cocl.us/Geospatial_data, from coursera class with the latitude and longitude coordinates 
of the  neighborhoods is also used. FourSquare API is used to explore 
venues of the neighborhoods by utilizing previous coordinates.

By querying the FourSquare API with coordinates of neighborhoods, one gets nearby venues including restaurants.
With the assumption that other venues drive interest in restaurants, we will analyze what neighborhoods with
a high number of restaurants have in common.

The FourSquare API endpoint we'll use is the explore one. https://api.foursquare.com/v2/venues/explore
By giving a coordinate and a radius, it returns a list of nearby venues in json format with its name, location in
latitude/longitude and category. It has extra information like photos, but we will not need those. For example,
at lat 43.753259 and long -79.329656 with a radius of 500 meters, explore endpoint returns a json that looks like:

{'meta': {'code': 200, 'requestId': '5fd520abe95789054963179c'}, 'response': {'warning'....
... 'venue': {'id': '4e8d9dcdd5fbbbb6b3003c7b', 'name': 'Brookbanks Park', 
'location': {'address': 'Toronto', 'lat': 43.751976046055574, 'lng': -79.33214044722958, ...
'categories': [{'id': '4bf58dd8d48988d163941735', 'name': 'Park', 'pluralName': 'Parks', ...
'venue': {'id': '4cb11e2075ebb60cd1c4caad', 'name': 'Variety Store', 
'location': {'address': '29 Valley Woods Road', 'lat': 43.75197441585782, ... 
'categories': [{'id': '4bf58dd8d48988d1f9941735', 'name': 'Food & Drink Shop', 'pluralName': 'Food & Drink Shops',... 

By parsing the json we can get venues Brookbanks Park and Variety Store.
