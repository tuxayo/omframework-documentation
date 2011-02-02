.. _mapserver_postgis:

###################
mapserver shapefile
###################

Cette page presente les differentes manieres d afficher un shapefile ::

    MAP
    chemin vers le shapefile, relatif au chemin SHAPEPATH
    
        LAYER
            L'utilisation d'une source WMS nécéssite la présence d'un bloc PROJECTION dans le bloc 
            MAP ainsi que l'utilisation d'un bloc METADATA (dans le bloc WEB et dans les blocs LAYER) 
            qui va préciser la requête faite au serveur. 
    END

Exemple ::

    MAP
    NAME "Test serveur distant WMS"
    SIZE 400 400
    EXTENT -74 40.6 -73.8 41
    STATUS ON
    UNITS METERS
    
    WEB
        IMAGEPATH "c:/ms4w/tmp/ms_tmp/"
        IMAGEURL "/ms_tmp/"
    END
    
        LAYER
          NAME "New York"
          TYPE RASTER
          STATUS DEFAULT
          CONNECTIONTYPE WMS
          CONNECTION "http://terraservice.net/ogcmap.ashx?"
          METADATA
            "wms_srs"             "EPSG:4326"
            "wms_name"            "DRG"
          "wms_server_version"  "1.1.1"
            "wms_format"          "png"
          END
        END
    
    END