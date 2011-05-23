.. _format:

######
format
######


ce chapitre se propose de decrire les divers formats de transfert avec openLayers
tab_sig_point.php n'utilise que le format json et wkt. Les autres formats sont donnés
à titre indicatif.

===    
txt
===

exemple : separateur par tab ::

    point		n	title	description	icon			url
    43.68485,4.63243	201	la gare d arles		arles/university.png	http://localhost/cimetiere/pontarlier.html
    43.68585,4.63343	202	college mistral		arles/university.png	
    43.68385,4.63143	203	college jlb		arles/university.png

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

=======
geojson
=======


Le format JSON correspond à la notation {paramètre : valeur, paramètre : valeur} de 
JavaScript, qui a l’avantage de ne pas nécessiter de conversion (« parsing »), puisqu’il est 
directement intégré par le moteur d’exécution JavaScript du navigateur

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


===
kml
===

exemple ::

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

======
GeoRSS
======

marqueur cliquable

uniquement point ::

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


couche GML sous classe vector
affichage de vecteurs

===
gml
===

v2::

    <gml:featureMember xmlns:gml="http://www.opengis.net/gml" xsi:schemaLocation="http://www.opengis.net/gml 
		http://schemas.opengis.net/gml/2.1.2/feature.xsd" 
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
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

===
gpx
===

exemple reseau de bus de pontarlier ::

    <?xml version='1.0' encoding='UTF-8'?>
    <gpx version="1.1" creator="JOSM GPX export" xmlns="http://www.topografix.com/GPX/1/1"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
        xsi:schemaLocation="http://www.topografix.com/GPX/1/1 http://www.topografix.com/GPX/1/1/gpx.xsd">
      <metadata>
        <bounds minlat="46.9018053" minlon="6.3345715" maxlat="46.9203521" maxlon="6.3649683" />
      </metadata>
      <trk>    <trkseg>
          <trkpt lat="46.911733" lon="6.3478355">
            <time>2009-12-26T16:32:39Z</time>
          </trkpt>
          <trkpt lat="46.9117632" lon="6.3478243">
            <time>2009-12-26T16:32:39Z</time>
          </trkpt>
          <trkpt lat="46.9117858" lon="6.3478078">
            <time>2009-12-26T16:32:39Z</time>
          </trkpt>
          <trkpt lat="46.9118187" lon="6.3477649">
            <time>2009-12-26T16:32:40Z</time>
          </trkpt>
          <trkpt lat="46.9118438" lon="6.3476903">
            <time>2009-12-26T16:32:39Z</time>
          </trkpt>
          <trkpt lat="46.9118475" lon="6.3476453">
            <time>2009-12-26T16:32:39Z</time>
          </trkpt>
          <trkpt lat="46.911844" lon="6.3476013">
            <time>2009-12-26T16:32:39Z</time>
          </trkpt>
        ...
        </trkseg>
      </trk>
      <trk>    <trkseg>
          <trkpt lat="46.9121278" lon="6.3600624">
            <time>2009-03-11T14:14:38Z</time>
          </trkpt>
          <trkpt lat="46.9116434" lon="6.3600321">
            <time>2009-03-11T14:14:40Z</time>
          </trkpt>
         ...
        </trkseg>
      </trk>
    </gpx>