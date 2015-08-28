##Put "country specific maps" on different webpages
I'm currently working on a project where I have a website that has a webpage for each country in a long list of countries.  Currently each page has a static map image for each of the different countries.  I would like to replace these static maps with an interactive map window.  When a user goes to a specific country webpage, I want the webpage to load, with the interactive map window zoomed into the extent of that specific country.

Currently I'm playing with the HERE maps API.  This API has a method which can be called to adjust a map windows extent called (setViewBounds)[https://developer.here.com/api-explorer/maps-js/maps/map-using-view-bounds]
