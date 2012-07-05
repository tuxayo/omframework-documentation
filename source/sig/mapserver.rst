.. _mapserver:

#########
Mapserver
#########

mapserver n est pas utilise et il a été préféré qgis_server dans les deploiements openMairie


Ce chapitre propose quelques principes de map server 


Principes
=========


MapServer permet de générer des cartes à partir de données diverses et de fichiers de configuration,
qui contiennent les paramètres décrivant la façon dont les données doivent être présentées, les mapfiles. 


Installation UBUNTU
===================

Verifiez que les depots Universe et Multiverse font partie de vos sources de mise a jour. 
installer les paquets suivants
(obligatoire)

- cgi-mapserver 

- mapserver-bin 

- mapserver-doc


*** creer un repertoire avec droit d ecriture pour www.data pour stocker les images crees par mapinfo
    var/www/tmp/ pour ubuntu
    ou changer le chemin de ce repertoire dans les openmairie_cimetiere/sig/map/xxxx.map ::
    
        WEB
            IMAGEPATH "/var/www/tmp/" 
            IMAGEURL "/tmp/" 
        END

- ATTENTION Verifier le chemin dans  openmairie_cimetiere/sig/var.inc qui doit correspondre a votre installation


getCapabilities
===============

http://vmap0.tiles.osgeo.org/wms/vmap0?request=GetCapabilities&version=1.0.0&service=WMS

renvoi un fichier xml avec

- la version du serveur et les installs ::

	<WMT_MS_Capabilities version="1.0.0">
	<!--
	MapServer version 5.0.3
            OUTPUT=GIF
            OUTPUT=PNG
            OUTPUT=JPEG
            OUTPUT=WBMP
            OUTPUT=SVG
            SUPPORTS=PROJ
            SUPPORTS=AGG
            SUPPORTS=FREETYPE
            SUPPORTS=WMS_SERVER
            SUPPORTS=WMS_CLIENT
            SUPPORTS=WFS_SERVER
            SUPPORTS=WFS_CLIENT
            SUPPORTS=WCS_SERVER
            SUPPORTS=FASTCGI
            SUPPORTS=THREADS
            SUPPORTS=GEOS
            INPUT=EPPL7
            INPUT=POSTGIS
            INPUT=OGR
            INPUT=GDAL
            INPUT=SHAPEFILE 
	-->

- les referentiels ::

	<SRS>EPSG:4269 EPSG:4326 EPSG:900913</SRS>


-  les layers ::

	<Layer>
	<Name>basic</Name>

        
Analyse d'une requete WMS
=========================

Requete ::
    http://vmap0.tiles.osgeo.org/wms/vmap0
        ?LAYERS=coastline_01
        &FORMAT=image%2Fpng
        &SERVICE=WMS
        &VERSION=1.1.1
        &REQUEST=GetMap
        &STYLES=
        &EXCEPTIONS=application%2Fvnd.ogc.se_inimage
        &SRS=EPSG%3A900913&BBOX=20037508.3384,-10203463.614977,40075016.6776,9834044.724223
        &WIDTH=256
        &HEIGHT=256

parametre ::

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
