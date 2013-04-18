Installation and Getting Started
=============

First of all you should sign-up for a CloudMade API key, by following this [link][], if you have not done it before.

###To install run:

	gem install cloudmade.gem



Get tile from CloudMade Tile Server
=============

Class _TilesService_ allows you to get tile (image of map) from CloudMade Tile Server.

Lets request the tile that contains the point with latitude = 47.26117 longitude = 9.59882 with zoom_level = 10


	#Example usage of CloudMade's API
	require 'cloudmade'
	include CloudMade

	cm = Client.from_parameters('Your_API_Key')
	png = cm.tiles.get_tile(47.26117, 9.59882, 10)
	file = File.new('my_tile.png', 'w')
	file.write(png)
	file.close


Lets look at what each bit of code is doing here:

###1. First you should import CloudMade module
	require 'cloudmade'
	include CloudMade


The last line means you can use CloudMade module classes directly, without referencing the module name

###2. Setup connection

	cm = Client.from_parameters('Your_API_Key')

Your_API_Key - Replace this with your CloudMade API key

Client class allows you to communicate with different CloudMade services, like Geocoding, Tiles etc.

###3. Get tile

	png = cm.tiles.get_tile(47.26117, 9.59882, 10)


_get_tile_ is a TileService's method, which returns a tile image.

###4. Save tile into file

	file = File.new('my_tile.png', 'w')
	file.write(png)
	file.close

=======
Geocoding - find geoobjects like city, street or point of interests
=============

Lets find Potsdamer Platz in Berlin,Germany and also let's find the closest pub to the point with latitude = 51.66117, longitude = 13.37654

	#Example usage of CloudMade's API

	require 'cloudmade'
	include CloudMade

	cm = Client.from_parameters('Your_API_Key')

	results = cm.geocoding.find('Potsdamer Platz, Berlin, Germany')
	if results.found then
	  geoobject = results.results[0]
	  puts geoobject.properties
	  puts geoobject.centroid
	end

	pub = cm.geocoding.find_closest('pub', 51.66117, 13.37654)
	puts pub.properties
	puts pub.centroid
Lets look at what each bit of code is doing here. After initialization you have a cm object (CloudMade client) connected to the CloudMade service.

###1. First you should import CloudMade module

	require 'cloudmade'
	include CloudMade
The last line means you can use CloudMade module classes directly, without referencing the module name

###2. Setup connection

	cm = Client.from_parameters('Your_API_Key')
Your_API_Key - Replace this with your CloudMade API key

Client class allows you to communicate with different CloudMade services, like Geocoding, Tiles etc.

###3. Find Potsdamer Platz in Berlin,Germany

	results = cm.geocoding.find('Potsdamer Platz, Berlin, Germany')
find is method of the GeocodingService, which finds roads, POIs, and other objects by their name and additional parameters (bounding box, type of object etc)

###4. Display properties and centroid of found geoobject

	if results.found then
	  geoobject = results.results[0]
	  puts geoobject.properties
	  puts geoobject.centroid
	end
###5. Find closest pub to point with latitude = 51.66117 longitude = 13.37654,
and display its properties and centroid

	pub = cm.geocoding.find_closest('pub', 51.66117, 13.37654)
	puts pub.properties
	puts pub.centroid

Routing - finds routes
=============

Lets find the shortest route by car between two points.

	#Example usage of CloudMade's API

	require 'cloudmade'
	include CloudMade

	cm = Client.from_parameters('Your_API_Key')

	route = cm.routing.route(
	    Point.new(47.25976, 9.58423),
	    Point.new(47.66117, 9.99882)
	)

	puts route.summary.total_distance
	puts route.summary.start_point
	puts route.summary.end_point
Lets look at what each bit of code is doing here:

###1. First you should import CloudMade module

	require 'cloudmade'
	include CloudMade
The last line means you can use CloudMade module classes directly, without referencing the module name

###2. Setup connection

	cm = Client.from_parameters('Your_API_Key')
Your_API_Key - Replace this with your CloudMade API key

Client class allows you to communicate with different CloudMade services, like Geocoding, Tiles etc.

###3. Find find car shortest route beetwen two points

	route = cm.routing.route(
	    Point.new(47.25976, 9.58423),
	    Point.new(47.66117, 9.99882)
	)
route is method of the RoutingService class, which finds a route between two points. It returns an instance of class Route.

###4. Display routing summary

	puts route.summary.total_distance
	puts route.summary.start_point
	puts route.summary.end_point
_route.summary_ contains some statistical information about the route, like distance, and total time.

[link]: http://account.cloudmade.com/register
