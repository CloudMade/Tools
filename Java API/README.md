Installation and Getting Started
=============

First of all you should sign-up for a CloudMade API key, by following this [link][], if you have not done it before.

###To install run:

	gem install cloudmade.gem



Get tile from CloudMade Tile Server
=============

Lets request the tile that contains the point with latitude = 47.26117 longitude = 9.59882 with zoom_level = 10

	/*
	 * Example usage of CloudMade's API
	 */

	import java.io.FileNotFoundException;
	import java.io.FileOutputStream;
	import java.io.IOException;

	import com.cloudmade.api.CMClient;

	public class GetTile {
	    public static void main(String[] args) {
	        CMClient client = new CMClient("8ee2a50541944fb9bcedded5165f09d9");
	        byte[] tile = client.getTile(47.26117, 9.59882, 9, 1, 256);

	        try {
	            FileOutputStream fos = new FileOutputStream("tile.png");
	            fos.write(tile);
	            fos.close();
	        } catch (FileNotFoundException e) {
	            ...
	        } catch (IOException e) {
	            ...
	        }
	    }
	}
Lets look at what each bit of code is doing here:

###1. First you should import CMClient class

	import com.cloudmade.api.CMClient;
###2. Setup client

	CMClient client = new CMClient("Your_API_Key");
Your_API_Key - Replace this with your CloudMade API key

###3. Get tile

	byte[] tile = client.getTile(47.26117, 9.59882, 9, 1, 256);
getTile is a method that gets a tile with given latitude, longitude, zoom, styleId and size (image dimension in pixels, 256 or 64). The method returns a byte array of the PNG image for the specified map tile.

4. Save tile into file

FileOutputStream fos = new FileOutputStream("tile.png");
fos.write(tile);
fos.close();

Geocoding - find geoobjects like city, street or point of interests
=============

Lets find Potsdamer Platz in Berlin,Germany and also let's find the closest pub to the point with latitude = 51.66117 and longitude = 13.37654

	/*
	 * Example usage of CloudMade's API
	 */

	import com.cloudmade.api.CMClient;
	import com.cloudmade.api.geocoding.GeoResult;
	import com.cloudmade.api.geocoding.GeoResults;
	import com.cloudmade.api.geocoding.ObjectNotFoundException;
	import com.cloudmade.api.geometry.Point;

	public class FindClosest {
	    public static void main(String[] args) {
	        CMClient client = new CMClient("Your_API_Key");

	        GeoResults results = client.find("Potsdamer Platz, Berlin, Germany", 2, 0, null, true, true, true);
	        GeoResult result = results.results[0];

	        System.out.println(result.properties);
	        System.out.println(result.centroid);

	        try {
	            result = client.findClosest("pub", new Point(51.66117, 13.37654));

	            System.out.println(result.properties);
	            System.out.println(result.centroid);
	        } catch (ObjectNotFoundException e) {
	            ...
	        }
	    }
	}
Lets look at what each bit of code is doing here:

###1. First you should import CMClient, GeoResult, GeoResults, ObjectNotFoundException, Point classes

	import com.cloudmade.api.CMClient;
	import com.cloudmade.api.geocoding.GeoResult;
	import com.cloudmade.api.geocoding.GeoResults;
	import com.cloudmade.api.geocoding.ObjectNotFoundException;
	import com.cloudmade.api.geometry.Point;
###2. Setup client

	CMClient client = new CMClient("Your_API_Key");
Your_API_Key - Replace this with your CloudMade API key

###3. Find Potsdamer Platz in Berlin, Germany

	GeoResults results = client.find("Potsdamer Platz, Berlin, Germany", 2, 0, null, true, true, true);
	GeoResult result = results.results[0];
_find_ is a method that finds geoobject(s) that match the given query.

###4. Display properties and centroid of found geoobject

	System.out.println(result.properties);
	System.out.println(result.centroid);
###5. Find closest pub to point with latitude = 51.66117, longitude = 13.37654 and display its properties and centroid

	result = client.findClosest("pub", new Point(51.66117, 13.37654));

	System.out.println(result.properties);
	System.out.println(result.centroid);
_findClosest_ is a method that finds closest geoobject of the given type to a given point.

Routing - find route between two points
=============

Lets find the shortest route by car between two points.

	/*
	 * Example usage of CloudMade's API
	 */

	import com.cloudmade.api.CMClient;
	import com.cloudmade.api.CMClient.MeasureUnit;
	import com.cloudmade.api.CMClient.RouteType;
	import com.cloudmade.api.geometry.Point;
	import com.cloudmade.api.routing.Route;
	import com.cloudmade.api.routing.RouteNotFoundException;

	public class GetRoute {
	    public static void main(String[] args) {
	        CMClient client = new CMClient("Your_API_Key");

	        try {
	            Route route = client.route(
	                new Point(47.25976, 9.58423),
	                new Point(47.66117, 9.99882),
	                RouteType.CAR,
	                null,
	                null,
	                "en",
	                MeasureUnit.KM
	            );

	            System.out.println(route.summary.totalDistance);
	            System.out.println(route.summary.startPoint);
	            System.out.println(route.summary.endPoint);
	        } catch (RouteNotFoundException e) {
	            ...
	        }
	    }
	}
Lets look at what each bit of code is doing here:

###1. First you should import CMClient, CMClient.MeasureUnit, CMClient.RouteType, Point, Route, RouteNotFoundException classes

	import com.cloudmade.api.CMClient;
	import com.cloudmade.api.CMClient.MeasureUnit;
	import com.cloudmade.api.CMClient.RouteType;
	import com.cloudmade.api.geometry.Point;
	import com.cloudmade.api.routing.Route;
	import com.cloudmade.api.routing.RouteNotFoundException;
###2. Setup client

	CMClient client = new CMClient("Your_API_Key");
Your_API_Key - Replace this with your CloudMade API key

###3. Find car shortest route between two points

	Route route = client.route(
	    new Point(47.25976, 9.58423),
	    new Point(47.66117, 9.99882),
	    RouteType.CAR,
	    null,
	    null,
	    "en",
	    MeasureUnit.KM
	);
route is a method that finds a route between two points with the given parameters.

###4. Display routing summary

	System.out.println(route.summary.totalDistance);
	System.out.println(route.summary.startPoint);
	System.out.println(route.summary.endPoint);

[link]: http://account.cloudmade.com/register
