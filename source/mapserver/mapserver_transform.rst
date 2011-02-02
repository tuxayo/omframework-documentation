.. _mapserver_transform:

#########
transform
#########


La publication de services webs OGC repose elle aussi sur le mapfile.
MapServer implémente les normes WMS, WFS et WCS.

Pour publier des données selon la norme WMS, il convient de respecter un certain nombre de règles :

- ajout d'un bloc PROJECTION dans le bloc MAP et dans chaque bloc LAYER

- ajout d'un bloc METADATA dans le bloc WEB et dans chaque bloc LAYER.

========
Bloc map
========

L'indication d'un géoréférencement pour la carte produite par le mapfile nécessite un 
bloc PROJECTION.
Ce bloc contient les informations de référencement, qui peuvent être :


- une série de paramètres PROJ.4  http://proj.maptools.org/gen_parms.html 

- un code EPSG  http://www.epsg.org/. 


Par exemple ::
    
        PROJECTION 
          "proj=utm" 
          "ellps=GRS80" 
          "zone=15" 
          "north" 
          "no_defs" 
        END 
    ou 
        PROJECTION 
          "init=epsg:28992" 
        END 




exemple de transformationdans le bloc MAP :

projection affichee en RG93  de la couche en Lambert sud ::

    MAP
        PROJECTION
            "init=epsg:2154"
        END
        
        dans le bloc layer
        
        LAYER
            NAME ...
            PROJECTION
                "init=epsg:27572"
            END
            TYPE POLYGON
            DATE "departement.shp"
        END
    END

A voir DATA pour postgis : DATA  " ... USING SRID=27572"


Attention : mapserver utilise proj4 :

Voir dans le fichier /usr/share/proj/epsg

rechercher la projection 4326 sous linux en utilisant proj, fichier espg ::

    $ grep 4326 /usr/share/proj/epsg
      latitude longitude 4326

Ajouter une ligne si il manque la projection 900913 (spherical mercator)

## spherical Mercator Google Maps
<900913> +proj=merc +a=6378137 +b=6378137 +lat_ts=0.0 +lon_0=0.0 +x_0=0.0 +y_0=0 +k=1.0 +units=m +nadgrids=@null +no_defs <>


http://spatialreference.org/ref/

