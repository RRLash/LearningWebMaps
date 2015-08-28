##Putting "country specific maps" on different webpages
I'm currently working on a project where I have a website that has a webpage for each country in a long list of countries.  Currently each page has a static map image for each of the different countries.  I would like to replace these static maps with an interactive map window.  When a user goes to a specific country webpage, I want the webpage to load, with the interactive map window zoomed into the extent of that specific country.

Currently I'm playing with the [HERE Maps Javascript API](https://developer.here.com/frontpage).  This API has a method which can be called to adjust a map windows extent called [setViewBounds](https://developer.here.com/api-explorer/maps-js/maps/map-using-view-bounds).  This method allows a user to input a bounding box for a geographic location based on the latitude and longitude coordinates of the upper left (UL) corner and lower right (LR) corner of the bounding box. Specifically, the input is a text string of decimal degree (DD) values representing these coordinates formatted as `(DD UL Latitude, DD UL Longitude, DD LR Latitude, DD LR Longitude)` or `(42.3736,-71.0751,42.3472,-71.0408)`.

For the time being, I want to hard code the bounding box information for each country page into the HTML, so I need to lookup the bounding box string for each country.  To get this information, I'm going to use the [Nominatim](http://wiki.openstreetmap.org/wiki/Nominatim) geocoder, which returns a bounding box field when a placename is queried (as well as a range of other information).  Since placenames can represent a variety of geographic localities, but I'm only interested in the bounding box around country polygons, I can modify the Nominatim HTML query to filter my results to only localities which have a type of country (excluding placenames that might be cities, streetnames, or buisness names).  This query has the following format

```html
http://nominatim.openstreetmap.org/search?country=Canada&format=xml
```
  - "http://nominatim.openstreetmap.org/search?" is the geocoder URL
  - "country=Canada" filters the results list to only features of type=country and name=Canada
  - "&" the ampersand is a concatenation operator
  - "format=json" declares that the results should be returned in an XML format

When you enter the query string above into your internet browser, you will get the follow results:  

```XML
<searchresults timestamp="Fri, 28 Aug 15 21:34:22 +0000" attribution="Data © OpenStreetMap contributors, ODbL 1.0. http://www.openstreetmap.org/copyright" querystring="Canada" polygon="false" exclude_place_ids="127863191" more_url="http://nominatim.openstreetmap.org/search.php?format=xml&exclude_place_ids=127863191&accept-language=en-US,en;q=0.8&q=Canada">
<place place_id="127863191" osm_type="relation" osm_id="1428125" place_rank="4" boundingbox="41.6765556,83.3362128,-141.0027499,-52.323198" lat="61.0666922" lon="-107.9917071" display_name="Canada" class="boundary" type="administrative" importance="0.99909828419394" icon="http://nominatim.openstreetmap.org/images/mapicons/poi_boundary_administrative.p.20.png"/>
</searchresults>
```
I have broken out the geocoding results fields from the XML code block above into separate lines in the code block below:

```
<searchresults timestamp="Fri, 28 Aug 15 21:34:22 +0000" 
attribution="Data © OpenStreetMap contributors, ODbL 1.0. http://www.openstreetmap.org/copyright" 
querystring="Canada" polygon="false" exclude_place_ids="127863191" more_url="http://nominatim.openstreetmap.org/search.php?format=xml&exclude_place_ids=127863191&accept-language=en-US,en;q=0.8&q=Canada">
  <place place_id="127863191" 
  osm_type="relation" 
  osm_id="1428125" 
  place_rank="4" 
  boundingbox="41.6765556,83.3362128,-141.0027499,-52.323198" 
  lat="61.0666922" 
  lon="-107.9917071" 
  display_name="Canada" 
  class="boundary" 
  type="administrative" 
  importance="0.99909828419394" 
  icon="http://nominatim.openstreetmap.org/images/mapicons/poi_boundary_administrative.p.20.png"/>
</searchresults>


```
From these results, I am then able to manually copy and paste the bounding box text string from the "boundingbox" field.
