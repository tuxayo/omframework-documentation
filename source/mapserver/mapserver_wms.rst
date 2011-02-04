.. _mapserver_wms:

#############
mapserver wms
#############


Le protocole WMS (Web Map Service) permet à un client d'obtenir une carte créée
à la demande par un serveur à l'aide d'une url normalisée
 
Définition : WMS :
Un serveur WMS doit répondre à 3 requêtes types :
- GetCapabilities
- GetMap
- GetFeatureInfo
  
 
La requête GetCapabilities permet au client d'obtenir sous forme d'un fichier XML le détail
des services fournis par le serveur WMS. Cette information est nécessaire pour formuler
correctement les requêtes destinées à récupérer les données géographiques.

Elle permet de connaître le nom des couches de données, les systèmes de coordonnées
dans lesquels on peut les obtenir, la sémantique disponible. 


Ce qui suit est une requête adressée au serveur français geosignal ::
 
    http://www.geosignal.org/cgi-bin/wmsmap?version=1.1.1&service=WMS&request=GetCapabilities 

Le serveur renvoie le fichier XML

La requête GetMap permet de récupérer une carte créée dynamiquement par le serveur WMS.
Il est nécessaire de spécifier un certain nombre de paramètres pour décrire la carte que
l'on souhaite obtenir ainsi que le montre la requête suivante ::

http://www.geosignal.org/cgi-bin/wmsmap?version=1.1.1&service=WMS&request=GetMap
    &SRS=EPSG:27582
    &BBOX=500000,2500000,600000,2600000
    &WIDTH=600
    &HEIGHT=600
    &LAYERS=Autoroutes,Nationales,Departementales
    &STYLES=&FORMAT=image/jpeg 

qui permet d'obtenir l'image JPEG 


La requête GetFeatureInfo permet d'obtenir les informations attributaires portées
par le (ou les) objet(s) localisé(s) là où on a cliqué sur la carte.
A noter que cette requête ne peut aboutir que pour les couches renseignées
comme queryable par la requête GetCapabilities. 
 
L'exemple suivant montre comment récupérer la sémantique d'une autoroute qui passe par le centre de notre carte
(la carte mesure 600 pixels de côté et on clique sur le point de coordonnées 300, 300) ::

    http://www.geosignal.org/cgi-bin/wmsmap?
            version=1.1.1
            &service=WMS
            &request=GetFeatureInfo
            &WIDTH=600&HEIGHT=600
            &srs=EPSG:27582
            &BBOX=500000,2500000,600000,2600000&X=300&Y=300
            &QUERY_LAYERS=Autoroutes&LAYERS=Autoroutes 

Le serveur renvoie un fichier texte



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
    



