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

=======
geojson
=======

fichier json polygone
=====================
$ find /var/www/ -name "*json"
*
$ less /var/www/ol_workshop/openlayers/examples/data/poly.json ::

format 2 polygones ::

    {
        "type": "FeatureCollection",
        "features": [
            {"type":"Feature",
             "id":"OpenLayers.Feature.Vector_1489",
             "properties":{},
             "geometry":
                {"type":"Polygon",
                 "coordinates":
                    [[[-109.6875, 63.6328125],
                      [-112.5, 35.5078125],
                      [-85.078125, 34.8046875],
                      [-68.90625, 39.7265625],
                      [-68.203125, 67.1484375],
                      [-109.6875, 63.6328125]
                ]]},
                "crs":
                    {"type":"OGC",
                     "properties":{"urn":"urn:ogc:def:crs:OGC:1.3:CRS84"}}
                    },
            {"type":"Feature",
             "id":"OpenLayers.Feature.Vector_1668",
             "properties":{},
             "geometry":
                {"type":"Polygon",
                 "coordinates":
                 [[[-40.78125, 65.0390625],
                   [-40.078125, 34.8046875],
                   [-12.65625, 25.6640625],
                   [21.09375, 17.2265625],
                   [22.5, 58.0078125],
                   [-40.78125, 65.0390625]
                 ]]},
                 "crs":
                    {"type":"OGC",
                     "properties":{"urn":"urn:ogc:def:crs:OGC:1.3:CRS84"}
                    }
            }
        ]
    }

format triangle  ::

    {"type":"Feature",
    "id":"OpenLayers.Feature.Vector_138",
    "properties":{},
    "geometry":
        {"type":"Polygon",
        "coordinates":
            [
                [
                    [-11.109377145767, 51.70313000679],
                    [19.124997854233, 19.35938000679],
                    [43.031247854233, 44.67188000679],
                    [-11.109377145767, 51.70313000679]
                ]
            ]
        },
        "crs":{
            "type":"OGC",
            "properties":{"urn":"urn:ogc:def:crs:OGC:1.3:CRS84"}
        }
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

<item xmlns="http://backend.userland.com/rss2">
    <title></title>
    <description></description>
    <georss:polygon xmlns:georss="http://www.georss.org/georss">
            51.70313000679 -11.109377145767
            19.35938000679 19.124997854233
            44.67188000679 43.031247854233
            51.70313000679 -11.109377145767
    </georss:polygon>
</item>

==========================
format atom georss et gml
=========================


<atom:entry xmlns:atom="http://www.w3.org/2005/Atom">
<atom:title>untitled</atom:title>
    <georss:where xmlns:georss="http://www.georss.org/georss">
        <gml:Polygon xmlns:gml="http://www.opengis.net/gml">
            <gml:exterior>
                <gml:LinearRing>
                    <gml:posList>
                        51.70313000679 -11.109377145767
                        19.35938000679 19.124997854233
                        44.67188000679 43.031247854233
                        51.70313000679 -11.109377145767
                    </gml:posList>
                </gml:LinearRing>
            </gml:exterior>
        </gml:Polygon>
    </georss:where>
</atom:entry>




couche GML sous classe vector
affichage de vecteurs

gml v2  ::

    <gml:featureMember xmlns:gml="http://www.opengis.net/gml" xsi:schemaLocation="http://www.opengis.net/gml http://schemas.opengis.net/gml/2.1.2/feature.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <feature:feature xmlns:feature="http://example.com/feature">
            <feature:geometry>
                <gml:Polygon>
                    <gml:outerBoundaryIs>
                        <gml:LinearRing>
                            <gml:coordinates decimal="." cs=", " ts=" ">
                                -11.109377145767, 51.70313000679
                                19.124997854233, 19.35938000679
                                43.031247854233, 44.67188000679
                                -11.109377145767, 51.70313000679
                            </gml:coordinates>
                        </gml:LinearRing>
                    </gml:outerBoundaryIs>
                </gml:Polygon>
            </feature:geometry>
        </feature:feature>
    </gml:featureMember>

gml v3 ::

    <gml:featureMember xmlns:gml="http://www.opengis.net/gml"
                        xsi:schemaLocation="http://www.opengis.net/gml http://schemas.opengis.net/gml/3.1.1/profiles/gmlsfProfile/1.0.0/gmlsf.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <feature:feature xmlns:feature="http://example.com/feature">
        <feature:geometry>
                <gml:Polygon>
                    <gml:exterior>
                        <gml:LinearRing>
                            <gml:posList>
                                -11.109377145767 51.70313000679
                                19.124997854233 19.35938000679
                                43.031247854233 44.67188000679
                                -11.109377145767 51.70313000679
                            </gml:posList>
                        </gml:LinearRing>
                    </gml:exterior>
                </gml:Polygon>
            </feature:geometry>
        </feature:feature>
    </gml:featureMember>





===
wkt
===


triangle ESPG 4326 ::

    POLYGON
        (
            (
            -11.109377145767 51.70313000679,
            19.124997854233 19.35938000679,
            43.031247854233 44.67188000679,
            -11.109377145767 51.70313000679
            )
        )

meme triangle spherical mercator ::

    POLYGON
        (
            (-1236690.2067251 6746624.52153,
             2128985.0222593 2197293.1218976,
             4790216.5986656 5570012.7757669,
             -1236690.2067251 6746624.52153
            )
    )

===
kml
===

<kml xmlns="http://earth.google.com/kml/2.0">
    <Folder>
        <name>OpenLayers export</name>
        <description>Exported on Thu Jan 20 2011 09:11:21 GMT+0100 (CET)</description>
        <Placemark>
            <name>OpenLayers.Feature.Vector_138</name>
            <description>No description available</description>
            <Polygon>
                <outerBoundaryIs>
                    <LinearRing>
                        <coordinates>
                            -11.109377145767,51.70313000679
                            19.124997854233, 19.35938000679
                            43.031247854233, 44.67188000679
                            -11.109377145767, 51.70313000679
                        </coordinates>
                    </LinearRing>
                </outerBoundaryIs>
            </Polygon>
        </Placemark>
    </Folder>
</kml>

