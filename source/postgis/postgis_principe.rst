.. _postgis_principe:



Ce chapitre propose les principes de map server.


principes
=========


PostGIS ajoute à postgresql :
- ses types de données
- ses fonctions,  
-  deux tables utilitaires : geometry_colums et spatial_ref_sys.
geometry_colums sert à indiquer au logiciel quels sont les champs contenantdes types
géographiques dans chacune des tables.
spatial_ref_sys contient les paramètres des systèmes de projection  supportés et sert en interne au logiciel. 

Cf. : http://postgis.refractions.net/documentation/manual-1.3/ch06.html 

Le principe est donc de pouvoir : ::

    -  stocker des informations géographiques en tenant compte de ces 
       caractéristiques particulières (projection, dimensions, etc.), 
    -  réaliser des opérations de type SIG comme des mesures de distance, 
       dʼintersection, 
    -  créer de nouvelles géométries en les décrivant dans un format reconnu, ou 
       comme résultat dʼopérations sur des géométries existantes (arithmétiques), 
    -  retourner des informations géographiques (géométries et/ou attributaires) selon 
       un format précis, sur le réseau. 

Installation de postgis
=======================

* sur UBUNTU installer les paquets postgis 
- postgis 
- postgresql-8.3.postgis

* Verification de l install postgresql-postgis

postgre> select version() ::

    "PostgreSQL 8.3.9 on i486-pc-linux-gnu, compiled by GCC gcc-4.3.real (Ubuntu 4.3.3-5ubuntu4) 4.3.3" 

postgre> show server_version ::
    
    "8.3.9" 

postgre> show data_directory ::

    "/var/lib/postgresql/8.3/main"
    
     
* version proj et geao 

postgre > select postgis_full_version() ::

"POSTGIS="1.3.5" GEOS="3.1.0-CAPI-1.5.0" PROJ="Rel. 4.6.1, 21 August 2008" USE_STATS (procs from 1.3.3 need upgrade)"


Paramétrage d'une base en postgis
=================================

Exemple : Installation de la base opencimetiere avec postgis

- creer la base opencimetiere (si elle n'est pas deja creee)

- postgre> create language "plpgsql" 

- executer (version postgis <1.5) la requete lwpostgis.sql -> fonction postgis
  ou executer (version postgis >= 1.5) la requete /usr/share/postgresql/8.3/contrib/postgis-1.5/postgis.sql 

* executer spatial_ref_sys .sql qui remplit la table de donnees spatial_ref_sys 

* VERIFICATION : les tables suivantes sont presentes ::

    * table geometry_columns : index des geometries (vide) 
    * table spation_ref_sys : liste des references spatiales (3162 lignes environ)

* executer les scripts d'initialisation de la base opencimetiere
    * data/pgsql/init.sql
    * data/pgsql/initsig.sql
    * data/pgsql/initsig_data.sql (optionnel) jeu de donnees



acces pgsql en console

$ psql -l

        Liste des bases de données
      Nom      | Propriétaire | Encodage  
---------------+--------------+-----------
 alaska        | postgres     | UTF8
 cadastre      | postgres     | SQL_ASCII
 opencimetiere | postgres     | SQL_ASCII
 openelec      | postgres     | SQL_ASCII
 openelec1     | postgres     | SQL_ASCII
 openerp       | postgres     | SQL_ASCII
 openfoncier   | postgres     | SQL_ASCII
 openmairie    | postgres     | SQL_ASCII
 postgres      | postgres     | UTF8
 sig           | postgres     | SQL_ASCII
 template0     | postgres     | UTF8
 template1     | postgres     | UTF8
 xx            | postgres     | SQL_ASCII
(13 lignes)

$ psql opencimetiere
Bienvenue dans psql 8.3.11, l'interface interactive de PostgreSQL.

    \h pour l'aide-mémoire des commandes SQL
    \? pour l'aide-mémoire des commandes psql
    \g ou point-virgule en fin d'instruction pour exécuter la requête
    \q pour quitter

