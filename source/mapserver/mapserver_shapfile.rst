.. _mapserver_postgis:

###################
mapserver shapefile
###################

Cette page presente les differentes manieres d afficher un shapefile ::

    MAP
    chemin vers le shapefile, relatif au chemin SHAPEPATH
    
        LAYER
        
            DATA "fichier.shp" (extension .shp est inutile)
        
            FILTER permet de filtrer les objets LAYER, c'est à dire avant le 
            traitement des blocs CLASS qui peuvent eux aussi comporter une sélection. 
        
            JOIN (bloc secondaire)
            (jointure) une table dbf de données attributaires,
            Ces  données deviennent ainsi interrogeables par MapServer en mode Query. 
        END
    END

Exemple ::

    MAP
    NAME "Europe en bleu"
    SIZE 400 400
    STATUS ON
    EXTENT 3852699.9709013 -260100.031703873 4159699.98477761 15299.9461098508
    UNITS METERS
    SHAPEPATH "var/www/formation_ol/mapserver/analyse/"
        
    WEB
      IMAGEPATH 'var/www/formation_ol/mapserver/ms_tmp/'
      IMAGEURL  '/ms_tmp/'
    END
    
    LAYER
        NAME "Région"
        TYPE POLYGON
        STATUS ON
        DATA "mp_sports"
        CLASSITEM "nom"
        CLASS
            EXPRESSION "TOULOUSE"
            STYLE
                COLOR 20 10 110
                OUTLINECOLOR 200 200 200
            END
        END
        CLASS
            EXPRESSION (([FOOTBALL] >= 1 AND [RUGBY] >= 1))
            STYLE
                COLOR 20 10 110
                OUTLINECOLOR 200 200 200
            END
        END
       	CLASS
		EXPRESSION (length ('[nom]') > 10)
            STYLE
                COLOR 20 10 110
                OUTLINECOLOR 200 200 200
            END
        END
        CLASS
            STYLE
                COLOR 255 255 255
                OUTLINECOLOR 200 200 200
            END
        END
    END
    
    END
    