.. _getcapabilities:

###############
getCapabilities
###############

http://vmap0.tiles.osgeo.org/wms/vmap0?request=GetCapabilities&version=1.0.0&service=WMS

renvoi un fichier xml avec

*** la version du serveur et les installs

	<WMT_MS_Capabilities version="1.0.0">
	<!--
	MapServer version 5.0.3 OUTPUT=GIF OUTPUT=PNG OUTPUT=JPEG OUTPUT=WBMP OUTPUT=SVG SUPPORTS=PROJ SUPPORTS=AGG SUPPORTS=FREETYPE SUPPORTS=WMS_SERVER 		SUPPORTS=WMS_CLIENT SUPPORTS=WFS_SERVER SUPPORTS=WFS_CLIENT SUPPORTS=WCS_SERVER SUPPORTS=FASTCGI SUPPORTS=THREADS SUPPORTS=GEOS INPUT=EPPL7 		INPUT=POSTGIS INPUT=OGR INPUT=GDAL INPUT=SHAPEFILE 
	-->

*** les referentiels

	<SRS>EPSG:4269 EPSG:4326 EPSG:900913</SRS>


***  les layers

	<Layer>
	<Name>basic</Name>


