.. _layer_osm:

#########
layer xyz
#########

Le but est de solliciter moins le serveur en ne recalculant pas chaque fois l image envoye

X, Y = coordonnées de la grille de tuile à afficher 
z = niveau de zoom

voir constructeur openLayer.Layer.XYZ

===
OSM
===

constructeur : openLayer.Layer.osm : acces au tuile osm



projection osm = 900913 (systeme de projection mercator

=======
Methode
=======

creer 2 systemes de references en creant un objet openLayers.Projection
 	4326 : geographique longitude / latitude
	900913 ::

	var geographic = new OpenLayers.Projection("EPSG:4326");
	var mercator = new OpenLayers.Projection("EPSG:900913");


transformer les cooredonnees geographique d arles en mercator ::

	var world = new OpenLayers.Bounds(-180, -89, 180, 89).transform(geographic, mercator);
	var arles = new OpenLayers.LonLat(4.632438,43.684858).transform(geographic, mercator);

passage des options au constructeur map ::

	var options = {
		projection: mercator,
		units: "m",
		maxExtent: world
	};
	var map = new OpenLayers.Map("map-id", options);


creation de la couche osm ::

	var osm = new OpenLayers.Layer.OSM();
	map.addLayer(osm);



=============
debug firefox
=============

get des images osm (toutes les tuiles)

http://tile.openstreetmap.org/12/2097/1491.png

Date	Wed, 12 Jan 2011 10:46:20 GMT
Server	Apache/2.2.8 (Ubuntu)
Etag	"6954eb45fe88e94647801bdc22f8fd25"
Expires	Thu, 13 Jan 2011 10:46:34 GMT
Cache-Control	max-age=86414
X-Cache	MISS from konqi.openstreetmap.org
X-Cache-Lookup	MISS from konqi.openstreetmap.org:3128
Via	1.1 konqi.openstreetmap.org:3128 (squid/2.7.STABLE7)
Connection	keep-alive

Requête
Host	tile.openstreetmap.org
User-Agent	Mozilla/5.0 (X11; U; Linux i686; fr; rv:1.9.2.13) Gecko/20101206 Ubuntu/9.10 (karmic) Firefox/3.6.13
Accept	image/png,image/*;q=0.8,*/*;q=0.5
Accept-Language	fr,fr-fr;q=0.8,en-us;q=0.5,en;q=0.3
Accept-Encoding	gzip,deflate
Accept-Charset	ISO-8859-1,utf-8;q=0.7,*;q=0.7
Keep-Alive	115
Connection	keep-alive
Referer	http://localhost/ol_workshop/map3.html
If-None-Match	"6954eb45fe88e94647801bdc22f8fd25"
Cache-Control	max-age=0
