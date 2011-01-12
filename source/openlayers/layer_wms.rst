.. _layer_wms:



#########
layer wms
#########


===
wms
===

web map service
- raster : image produite coté serveur
- vecteur : fournit par le serveur sous forme de données structurées et affichées par le navigateur

============
constructeur
============

openLayers.Layer.WMS herite de openLayers
il necessite 3 arguments :



var imagery = new OpenLayers.Layer.WMS(
				"Global Imagery",                       	nom de la couche
				"http://vmap0.tiles.osgeo.org/wms/vmap0",	url du serveur WMS
				{layers: "basic"}				objet contenant les propriétés de la requete wms
				);

Pour rajouter des nouvelles propriétés, utiliser le 3eme argument :
{layers: "coastline_01", format:"image/png"}    				affiche le layer coastline_01 et utilise le format png


=============
debug firefox
=============

Analyse d'une requete WMS (reseau -> tous)


Requete
=======
http://vmap0.tiles.osgeo.org/wms/vmap0	?LAYERS=coastline_01
				       	&FORMAT=image%2Fpng
                                       	&SERVICE=WMS
					&VERSION=1.1.1
                                       	&REQUEST=GetMap
                                       	&STYLES=
                                       	&EXCEPTIONS=application%2Fvnd.ogc.se_inimage
					&SRS=EPSG%3A900913&BBOX=20037508.3384,-10203463.614977,40075016.6776,9834044.724223
					&WIDTH=256
					&HEIGHT=256


parametre
=========

	BBOX	-20037508.34,-10203463.614977,-0.0007999911904335,9834044.724223
	EXCEPTIONS	application/vnd.ogc.se_inimage
	FORMAT	image/png
	HEIGHT	256
	LAYERS	coastline_01
	REQUEST	GetMap
	SERVICE	WMS
	SRS	EPSG:900913
	STYLES	
	VERSION	1.1.1
	WIDTH	256

entete
======

Réponse
Date	Wed, 12 Jan 2011 10:20:53 GMT
Server	Apache/2.2.9 (Debian) PHP/5.2.6-1+lenny9 with Suhosin-Patch
Keep-Alive	timeout=2, max=99
Connection	Keep-Alive
Transfer-Encoding	chunked
Content-Type	image/png

Requête
Host	vmap0.tiles.osgeo.org
User-Agent	Mozilla/5.0 (X11; U; Linux i686; fr; rv:1.9.2.13) Gecko/20101206 Ubuntu/9.10 (karmic) Firefox/3.6.13
Accept	image/png,image/*;q=0.8,*/*;q=0.5
Accept-Language	fr,fr-fr;q=0.8,en-us;q=0.5,en;q=0.3
Accept-Encoding	gzip,deflate
Accept-Charset	ISO-8859-1,utf-8;q=0.7,*;q=0.7
Keep-Alive	115
Connection	keep-alive


