Installation and Getting Started
=============

First of all you should sign-up for a CloudMade API key, by following this [link][], if you have not done it before.

	tar -xvf Geo-Cloudmade-0.1.tar.gz
	cd Geo-Cloudmade-0.1/
Go to the CPAN console:

	sudo cpan
Install JSON, YAML and LWP:

	install JSON
	install YAML
	install LWP
Exit from the CPAN console:

	exit
Then build the CloudMade Perl API:

	perl Makefile.PL
	make
	make test
	sudo make install

Get tile from CloudMade Tile Server
=============

Let's get the tile with central point latitude = 47.26117, longitude = 9.59882 and zoom_level = 10

	#Example usage of CloudMade's API
	use Geo::Cloudmade;
	#use api key for access to service
	my $geo = Geo::Cloudmade->new('Your_API_Key');
	my $tile = $geo->get_tile([47.26117, 9.59882], {zoom=>10, tile_size=>256});
	open (my $fh, '>', 'test.png') or die "Cannot open file $!\n";
	binmode $fh;
	print $fh $tile;
Your_API_Key - Replace this with your CloudMade API key

Geocoding - find geoobjects like city, street or point of interests
=============

Let's find Potsdamer Platz in Berlin, Germany and the closest pub to the point with latitude = 51.66117, longitude = 13.37654

	use Geo::Cloudmade;
	#use api key for access to service
	my $geo = Geo::Cloudmade->new('Your_API_Key');
	my @arr = $geo->find("Potsdamer Platz,Berlin,Germany", {results=>5, skip=>0});
	print $geo->error(), "\n" unless @arr;
	print "Number of results: ", scalar (@arr), "\n";
	foreach (@arr) {
	    print $_->name,":\n", $_->centroid->lat, "/", $_->centroid->long, "\n"
	}
	@arr = $geo->find_closest('pub', [51.66117, 13.37654], {return_geometry=>'True'});
	print $geo->error(), "\n" unless @arr;
	print "Number of results: ", scalar (@arr), "\n";
	foreach (@arr) {
	    print $_->name,":\n", $_->centroid->lat, "/", $_->centroid->long, "\n"
	}
Your_API_Key - Replace this with your CloudMade API key

Routing - find routes
=============

Let's find the shortest route by car between two points.

	use Geo::Cloudmade;
	#use api key for access to service
	my $geo = Geo::Cloudmade->new('Your_API_Key');
	my $route = $geo->get_route([47.25976, 9.58423], [47.66117, 9.99882], { type=>'car', method=>'shortest' } );
	print "Distance: ", $route->total_distance, "\n";
	print "Start: ", $route->start, "\n";
	print "End: ", $route->end, "\n";
	print "Route segments:\n";
	print join (',', @$_), "\n" foreach (@{$route->segments});

Your_API_Key - Replace this with your CloudMade API key

[link]: http://account.cloudmade.com/register
