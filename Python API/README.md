Getting Started
=============

First of all you should sign-up for a CloudMade API key, by following this [link][], if you have not done it before.

Api module
=============

	from cloudmade import api, geocoding, routing, utils

	cm = api.API(apikey='YOUR_API_Key', referrer='your-domain.com')
	sf = cm.find(geocoding.Query('San Francisco'))['features'][0]['centroid']['coordinates']
	palo_alto = cm.find(geocoding.Query('Palo Alto'))['features'][0]['centroid']['coordinates']
	route = cm.route(routing.Query(sf, palo_alto))
	zoom = 14
	start, end = [utils.latlon2tilenums(zoom, point) for point in (sf, palo_alto)]
	start, end = [cm.tile('1', 256, zoom, *point) for point in (start, end)]
	with open('startpoint.png', 'w') as f:
	    f.write(start)
	with open('endpoint.png', 'w') as f:
	    f.write(end)

Routing module
=============
###Routing in Amsterdam from Central Station to Rijksmusem (or somewhere near):


	from cloudmade import routing
	from pprint import pprint

	router = routing.Router(apikey='YOUR_API_Key', referer='your-domain.com')
	query = routing.Query([52.37, 4.89], [52.36, 4.88]).miles()
	result = router.route(query)
	pprint(result)

###Looking for a route between Kyiv and Kuznetsovsk with a stop in Zhitomir:

	from cloudmade import routing
	from pprint import pprint

	router = routing.Router(apikey='YOUR_API_Key', referer='your-domain.com')
	query = routing.Query().from_([50.45, 30.53]).to([51.34,
	25.87]).through([50.25, 28.66]).lang('ru')
	result = router.route(query)
	pprint(result)

Geocoding module
=============
###Looking for pubs around 500 meters from current point (randomly chosen for this example):
	from cloudmade import geocoding
	from pprint import pprint

	geocoder = geocoding.Geocoder(apikey='YOUR_API_Key', referer='your-domain.com')
	query = geocoding.Query('pub').around([51.51384, -0.10952], 500)
	result = geocoder.find(query)
	pprint(result)

###Looking for exactly 5 cinemas in bounding box including Berlin:
	from cloudmade import geocoding
	from pprint import pprint

	geocoder = geocoding.Geocoder(apikey='YOUR_API_Key', referer='your-domain.com')
	query = geocoding.Query().bbox([52.5884, 13.1788, 52.4745, 13.5381], True).object_type('cinema').limit(5)
	result = geocoder.find(query)
	pprint(result)

Tiles module
=============

###Simple tile request (this tile contains almost all San Francisco). The snippet saves the tile to a files called SF.png. Fire up your image viewer after you run this script and check out the tile!:
	from cloudmade import tiles

	tile = tiles.Tile('YOUR_API_Key', 'your-domain.com')
	data = tile.fetch('997', 256, 11, 327, 791)
	with open('SF.png', 'w') as f:
	    f.write(data)

[link]: http://account.cloudmade.com/register