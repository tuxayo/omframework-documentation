.. _mapserver_layer:



Ce chapitre decrit le bloc layer


##########
bloc layer
##########

le bloc layer est integre dans le blocMap

    MAP
        LAYER1 
        END
        LAYER2
        END
    END

Les blocs LAYER sont dessinés dans l'ordre du mapfile

=====================
Paramètres généraux : 
=====================

Les paramètres sont les suivants ::

    NAME : Nom de la couche (unique, max 20 caractères, entre guillemets) 
    
    GROUP : Groupe auquel le LAYER appartient.
            Utilisé dans les modèles HTML pour activer/désactiver les couches par groupes. 
    
    METADATA : Bloc secondaire utiliser pour stocker des paires nom – valeur. Utilisé 
               par les modèles HTML et en mode serveur WMS. 
    
    STATUS :
        Statut (visibilité) du layer.
        Valeurs : default(=visible), on, off
    
    TYPE :
    
        Type d'objet géométrique ou
        modalité selon laquelle la couche doit être dessinée.
        Valeurs : point|line|polygon|circle|annotation|raster|query.
            peut prendre une valeur différente du type géométrique des objets contenus
            (exemple une couche de polygones peut être représentée par des points 
            ce qui affichera les centroïdes des polygones) 
    
    MINSCALE : Échelle minimale à laquelle la couche sera dessinée.
    
    MAXSCALE : Idem pour l'échelle maximale. 
    
    SYMBOLESCALE : echelle des symboles et/ou les textes apparaissent à 
    leur taille normale. Ce paramètre permet un dimensionnement dynamique de ce type 
    d'objets selon l'échelle de la carte, dans les limites des deux paramètres précédents. 
    Obligatoire pour l'utilisation du paramètre SIZEITEM dans un bloc CLASS. 
    
    OPACITY : Degré de transparence de la couche, exprimé en pourcentage
              de 100 – opaque à 0 – totalement transparent. 
    
    OFFSITE : Le numéro d'index de la couleur d'une couche raster à traiter comme 
    transparent. Cela permet de ne garder que la région utile d'une couche raster. 
    
    POSTLABELCACHE: true / false 
                    couche après labels
                    false par défaut. 
    
    CLASSITEM : Nom de la colonne attributaire blocs CLASS. 
    
    LABELITEM : Nom de la colonne attributaire qui fournira le texte des étiquettes. 
    
    TEMPLATE : Nom du fichier modèle HTML qui prend en compte cette couche. 
               Obligatoire pour rendre cette couche interrogeable par requête,
               même si on n'utilise pas de modèle HTML. 
    
    DEBUG : Valeur On ou Off.
            Si le paramètre général LOG est défini, les messages de 
            débogage détaillés seront ajoutés au fichier de log,
            en plus des messages d'erreur. 
     
=======================
Paramètres de données :
=======================

Les connexions aux données sont étudiés plus precisemment dans les autres chapitres ::

    • Shapefiles voir mapserver_shapefile

    • Couches vectorielles accédées avec OGR 

    • Source serveurs de données
    
        CONNECTION. 
            ArcSDE
            PostGIS  voir mapserver_postgis
            Oracle Spatial 
            Web Map Services (WMS) voir mapserver_web 

    • Couches raster voir mapserver_raster


bloc class, layer et style voir mapserver_class

