.. _sig_outils:

######
outils
######


ce chapitre propose de décrire de manière sommaire les outils geographiques
pouvant être utiles à l extension d'om_sig ::

    GEOS pour le calcul geometrique
    GDAL pour les rasters
    OGR pour l'interrogation des données
    PROJ pour la projection

GEOS et PROJ sont utilisés par postgis et mapserver.

Voir exemple d'utilisation : http://zoo-project.org/


PROJ pour les projections
=========================
Version de proj ::

    $ proj -- version 

la table des projection est dans usr/share/proj/epsg ::

    # Google
    <900913> +proj=merc +lon_0=0 +k=1 +x_0=0 +y_0=0 +ellps=WGS84 +datum=WGS84 +units=m +no_defs<>

postgis utilise proj et  la table ref_spatial_sys


GDAL pour les rasters
=====================

installation ::

    $ sudo apt-get install gdal-bin
    $ gdalinfo --formats

    dependance de gdalinfo
    $ ldd usr/bin/gdalinfo
    $ apt-get install lib-gdal1-dev

    pour faire des tuiles
    apt-get install python_gdal
    gdap to tile ?
    

    format rasters
    
    Origine : point bas gauche
    largeur : pixelsize H
    longueur : pixelsize V
    taille du pixel
    pixel : x,y + altitude (z)

Exemple d'utilisation de gdal pour merger deux images :: 
    
    $ gdal_merge.py -o srtm_location.tif srtm_37_04.tif srtm_38_04.tif
    $ gdaldem hillshade srtm_location.tif shade.tif -z 5 -s 111120 -az 90


http://gdal.org/ogr/index.html


OGR pour l interogation de tout type de format
==============================================

liste des format accessibles parogr ::

    $ ogr2ogr --formats | grep d/w ou $ ogr2ogr --formats ::

  -> "ESRI Shapefile" (read/write)
  -> "MapInfo File" (read/write)
  -> "TIGER" (read/write)
  -> "S57" (read/write)
  -> "DGN" (read/write)
  -> "Memory" (read/write)
  -> "BNA" (read/write)
  -> "CSV" (read/write)
  -> "GML" (read/write)
  -> "GPX" (read/write)
  -> "KML" (read/write)
  -> "GeoJSON" (read/write)
  -> "Interlis 1" (read/write)
  -> "Interlis 2" (read/write)
  -> "GMT" (read/write)
  -> "SQLite" (read/write)
  -> "ODBC" (read/write)
  -> "PostgreSQL" (read/write)
  -> "MySQL" (read/write)
  -> "Geoconcept" (read/write)

    $ ogrinfo --formats
    $ history | grep odp ogr

    acces au sgbd postgres base "odp"
    $ ogrinfo -so "PG:dbname=odp"
    $ ogrinfo -so "PG:dbname=odp" odp | less

    acces a un fichier shape ::
    $ ogrinfo -so ./natural.shp
    
    faire une requete sur un shape
    $ ogrinfo -sql "SELECT type from natural" ./natural.shp  ogrinfo -so ./natural.shp
    
    requete fabriquant un shape ::
    $ ogr2ogr -sql "SELECT ST_Buffer(geom,10),ville,voie,complement from odp" test_data.shp "PG:dbname=odp"
    $ ogr2ogr -sql "SELECT ST_Buffer(geom,1),ville,voie,complement from odp" test_data2.shp "PG:dbname=odp"


http://dl.maptools.org/dl/php_ogr/php_ogr_documentation.html

GEOS 
====

calcul geometrique (voir requete postgis)
