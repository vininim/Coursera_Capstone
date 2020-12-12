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
