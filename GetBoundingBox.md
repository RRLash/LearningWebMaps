##Putting "country specific maps" on different webpages
I'm currently working on a project where I have a website that has a webpage for each country in a long list of countries.  Currently each page has a static map image for each of the different countries.  I would like to replace these static maps with an interactive map window.  When a user goes to a specific country webpage, I want the webpage to load, with the interactive map window zoomed into the extent of that specific country.

Currently I'm playing with the HERE maps API.  This API has a method which can be called to adjust a map windows extent called [setViewBounds](https://developer.here.com/api-explorer/maps-js/maps/map-using-view-bounds).  This method allows a user to input a bounding box for a geographic location based on the latitude and longitude coordinates of the upper left (UL) corner and lower right (LR) corner of the bounding box. Specifically, the input is a text string of decimal degree (DD) values representing these coordinates formatted as `(DD UL Latitude, DD UL Longitude, DD LR Latitude, DD LR Longitude)` or `(42.3736,-71.0751,42.3472,-71.0408)`.

For the time being, I want to hard code the bounding box information for each country page into the HTML, so I need to lookup the bounding box string for each country.  To get this information, I'm going to use the [Nominatim](http://wiki.openstreetmap.org/wiki/Nominatim) geocoder, which returns a bounding box field when a placename is queried (as well as a range of other information).  Since placenames can represent a variety of geographic localities, but I'm only interested in the bounding box around country polygons, I can modify the Nominatim HTML query to filter my results to only localities which have a type of country (excluding placenames that might be cities, streetnames, or buisness names).  This query has the following format

```
http://nominatim.openstreetmap.org/search?country=Canada&format=json&polygon=1
// "http://nominatim.openstreetmap.org/search?" is the geocoder URL
// "country=Canada" filters the results list to only features of type=country and name=Canada
// "&" the ampersand is a concatenation operator
// "format=json" declares that the results should be in a json format
