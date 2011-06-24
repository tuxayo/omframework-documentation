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
    43.68485,4.63243	201	la gare d arles		arles/university.png	http://localhost/xx.html
    43.68585,4.63343	202	college mistral		arles/university.png	
    43.68385,4.63143	203	college jlb		    arles/university.png

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
            ...
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

