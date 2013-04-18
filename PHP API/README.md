Getting Started
=============

First of all you should sign-up for a CloudMade API key, by following this [link][], if you have not done it before.

Get tile from CloudMade Tile Server
=============

Lets request the tile that contains the point with latitude = 47.26117, longitude = 9.59882 and zoom_level = 10.

	require 'Tile.php';

	$connection = new Connection('Your_API_Key');

	$tile = cm_get_tile($connection, 47.26117, 9.59882, 10, 1, 256);

	$f = fopen('file.png', 'w+');
	fprintf($f, '%s', $tile);
	fclose($f)

Your_API_Key - Replace this with your CloudMade API key

Geocoding - find geoobjects like city, street or point of interests
=============

Lets find Potsdamer Platz in Berlin,Germany and also let's find the closest pub to the point with latitude = 51.66117 and longitude = 13.37654

	require 'Geocoding.php';

	$results = cm_find($connection, "Potsdamer Platz,Berlin,Germany", 10, 0); 
	$result = $results->results[0];
	echo $result->properties_to_string() . '<br />' . $result->centroid->to_string() . '<br />';

	$result = cm_find_closest($connection, 'pub', new Point(array(51.66117, 13.37654)));
	echo $result->properties_to_string() . '<br />' . $result->centroid->to_string() . '<br />';

Routing - find route between two points
=============

Lets find the shortest route by car between two points.

	require_once 'Routing.php';

	$instructions = cm_route($connection, new Point(array(47.25976, 9.58423)), new Point(array(47.66117, 9.99882))); //$routing->route(new Point(array(47.25976, 9.58423)), new Point(array(47.66117, 9.99882)))->instruction();

	echo '<br />';

	for ($i = 0; $i < count($instructions); $i++)
	 echo $instructions[$i]->instruction . '<br />';

[link]: http://account.cloudmade.com/register
