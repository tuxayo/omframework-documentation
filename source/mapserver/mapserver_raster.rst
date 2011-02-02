.. _maps
erver_raster:

################
mapserver raster
################

Cette page presente l'afichage raster ::

Les sources de données raster composées de nombreux fichiers juxtaposés sont gérables en 
une seule fois par MapServer, sous la forme d'un tuilage (tiles).
Ce tuilage consiste en un fichier shape qui contient des objets polygone représentant l'extension de chaque tuile raster, 
et ayant comme attribut le nom du fichier.
Ce type de tuilage peut être produt en utilisant l'utilitaire gdaltindex distribué avec GDAL. Cela permet une gestion beaucoup plus rapide des 
grandes couvertures raster, notamment si les bitmaps sont eux aussi optimisés pour être lus à 
différentes résolutions . 

References :

http://mapserver.gis.umn.edu/docs/howto/   

http://www.gdal.org/

formats_list.html 
 
Motcles ::

    MAP
    chemin vers le shapefile, relatif au chemin SHAPEPATH
    SHAPEPATH
    
    LAYER
        DATA "nomdufichier_image" (eventuellement chemin)        
        TYPE raster        
        PROCESSING traitement de raster
        GDAL est capable de réaliser des traitements de rééchantillonage  
        SCALE mise à l'échelle
        DITHERING de tramage ()
        BAND selection de bande
    END

Exemple ::

    // exemple 1

    MAP
    IMAGETYPE      PNG24
    EXTENT        201621.496941 -294488.285333 1425518.020722 498254.511514
    SIZE           400 300
    SHAPEPATH      "."
    
    PROJECTION
     "init=epsg:2163"
    END
    
    WEB
      IMAGEPATH 'c:/ms4w/tmp/ms_tmp/'
      IMAGEURL  '/ms_tmp/'
    END
    
    LAYER
      NAME         modis
      DATA         "mod13a12000257evi.tif"
      STATUS       DEFAULT
      TYPE         RASTER
      OFFSITE      0 0 0
    
      PROJECTION
        "init=epsg:4326"
      END
    
    END
    
    END
    
    // exemple 2
    
    MAP
    IMAGETYPE      PNG24
    EXTENT        201621.496941 -294488.285333 1425518.020722 498254.511514
    SIZE           400 300
    SHAPEPATH      "."
    
    PROJECTION
     "init=epsg:2163"
    END
    
    WEB
      IMAGEPATH 'C:/ms4w/tmp/ms_tmp/'
      IMAGEURL  '/ms_tmp/'
    END
    
    LAYER
      NAME         modis
      DATA         "mod13a12000257evi.tif"
      STATUS       DEFAULT
      TYPE         RASTER
      OFFSITE      0 0 0
    
      PROJECTION
        "init=epsg:4326"
      END
    
      CLASSITEM "[pixel]"
      CLASS
        EXPRESSION ([pixel] < 32)
        COLOR 0 0 0
      END
      CLASS
        EXPRESSION ([pixel] >= 32 AND [pixel] < 128)
        COLOR 255 0 0 
      END
      CLASS
        EXPRESSION ([pixel] >= 128 AND [pixel] < 164)
        COLOR 0 255 0 
      END
      CLASS
        EXPRESSION ([pixel] >= 164 AND [pixel] < 256)
        COLOR 0 0 255
      END
    END