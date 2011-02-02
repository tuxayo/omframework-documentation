.. _maps
erver_ogc:

#############
mapserver ogc
#############

Cette page presente l'afichage ogc ::

La bibiothèque de fonctions OGR permet à MapServer de lire un grand nombre de 
formats SIG vectoriels :

Pour indiquer une source de données OGR, il faut utiliser la syntaxe suivante : 

    CONNECTIONTYPE OGR 

    CONNECTION [nom de la source] 

Ce dernier paramètre dépend du type de source, généralement il s'agit du chemin relatif au 
SHAPEPATH et du nom du fichier, ou du nom du répertoire (source ArcInfo coverage par 
exemple). Certains types de sources vectorielles sont organisées en couches multiples par 
fichier, il faut donc choisir la couche à utiliser avec le paramètre DATA [numéro/nom de la 
couche]. MapServer peut aussi récupérer en partie les éventuels styles d'affichage présents 
dans les couches, avec le paramètre STYLEITEM AUTO. Le paramètre FILTER est utilisable 
aussi avec ce type de données, comme pour les shapefiles. 

References :

http://www.gdal.org/ogr/ogr_formats.html

