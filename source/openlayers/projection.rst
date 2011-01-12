.. _projection:

##########
Projection
##########

			terre					EPSG	x/y		fournisseur
sphericalmercator 	sphere		tuiles raster		900913	en metre	osm, google, bing
wgs84			elipsoide	latitude longitude	4326	en degre ?			
		

==================
reference spatiale
==================


http://spatialreference.org/ref/

rechercher la projection 4326 sous linux en utilisant proj, fichier espg ::

    $ grep 4326 /usr/share/proj/epsg
      latitude longitude 4326