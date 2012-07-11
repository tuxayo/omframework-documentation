.. _format:

######
format
######


ce chapitre se propose de decrire les divers formats de transfert avec openLayers
tab_sig.php n'utilise que le format json et wkt. 

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




