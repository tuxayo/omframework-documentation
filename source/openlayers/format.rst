.. _format:

######
format
######

=======
ogr2ogr
=======

$ ogr2ogr --formats | grep d/w 
ou 
$ ogr2ogr --formats

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



=====================
fichier json polygone
=====================
$ find /var/www/ -name "*json"
*
$ less /var/www/ol_workshop/openlayers/examples/data/poly.json ::

    {
        "type": "FeatureCollection",
        "features": [
            {"type":"Feature", "id":"OpenLayers.Feature.Vector_1489", "properties":{}, "geometry":{"type":"Polygon", "coordinates":[[[-109.6875, 63.6328125], [-112.5, 35.5078125], [-85.078125, 34.8046875], [-68.90625, 39.7265625], [-68.203125, 67.1484375], [-109.6875, 63.6328125]]]}, "crs":{"type":"OGC", "properties":{"urn":"urn:ogc:def:crs:OGC:1.3:CRS84"}}},
            {"type":"Feature", "id":"OpenLayers.Feature.Vector_1668", "properties":{}, "geometry":{"type":"Polygon", "coordinates":[[[-40.78125, 65.0390625], [-40.078125, 34.8046875], [-12.65625, 25.6640625], [21.09375, 17.2265625], [22.5, 58.0078125], [-40.78125, 65.0390625]]]}, "crs":{"type":"OGC", "properties":{"urn":"urn:ogc:def:crs:OGC:1.3:CRS84"}}}
        ]
    }

======
marker
======

addMarker
uniquement point

=============
format GeoRSS
=============

couche GeoRSS
marqueur cliquable
uniquement point

===
GML
===

couche GML sous classe vector
affichage de vecteurs

======
vector
======

