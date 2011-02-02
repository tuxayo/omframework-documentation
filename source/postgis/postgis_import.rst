.. _postgis_import:



*** Recuperation fichier shp
-> 3 fichiers avec extension shp shx dbf
shp2pgsql -D -I /home/utilisateur/cadastreArles/13004/PARCELLE_area.shp parcelle | psql cadastre 
shp2pgsql -D -I /home/utilisateur/cadastreArles/13004/BATIMENT_area.shp batiment  | psql cadastre 
par defaut column_geometry = -1

shp2pgsql -s [srid] -I -D communes.shp | psql [base] 

exemple :

shp2pgsql -s 2154 -I -D DEPARTEMENT.SHP | psql postgis
