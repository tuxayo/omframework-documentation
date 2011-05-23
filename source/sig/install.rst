.. _install:

#######
install
#######

Pour faire fonctionner tab_sig_point.php, il faut :

- postgis

- openlayers qui est de base dans le framework en version 



postgis
=======

sur UBUNTU installer les paquets postgis ::

    - postgis 
    - postgresql-8.3.postgis

Verification de l install postgresql-postgis ::

    postgre> select version() 

    "PostgreSQL 8.3.9 on i486-pc-linux-gnu, compiled by GCC gcc-4.3.real (Ubuntu 4.3.3-5ubuntu4) 4.3.3" 

    postgre> show server_version 
    
    "8.3.9" 

    postgre> show data_directory 

    "/var/lib/postgresql/8.3/main"
    
     
version proj et geao ::

    postgre > select postgis_full_version() ::

    "POSTGIS="1.3.5" GEOS="3.1.0-CAPI-1.5.0" PROJ="Rel. 4.6.1, 21 August 2008" USE_STATS (procs from 1.3.3 need upgrade)"

Voir paragraphe "outils"


Paramétrage d'une base avec postgis
===================================

- creer la base openmairie (si elle n'est pas deja creee)

- postgre> create language "plpgsql" 

- executer (version postgis <1.5) la requete lwpostgis.sql -> fonction postgis
  ou executer (version postgis >= 1.5) la requete /usr/share/postgresql/8.3/contrib/postgis-1.5/postgis.sql 

- executer spatial_ref_sys .sql qui remplit la table de donnees spatial_ref_sys 

- VERIFICATION : les tables suivantes sont presentes ::

    * table geometry_columns : index des geometries (vide) 
    * table spation_ref_sys : liste des references spatiales (3162 lignes environ)

- executer les scripts d'initialisation de la base ::

    * data/pgsql/init.sql
    * data/pgsql/initsig.sql
    * data/pgsql/initsig_data.sql (optionnel) jeu de donnees


verification des bases : liste des bases en console ::

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
    
acces a opencimetiere ::

    $ psql opencimetiere
    
    Bienvenue dans psql 8.3.11, l'interface interactive de PostgreSQL.
        \h pour l'aide-mémoire des commandes SQL
        \? pour l'aide-mémoire des commandes psql
        \g ou point-virgule en fin d'instruction pour exécuter la requête
        \q pour quitter

exemple de selection des colones geometriques de la base "odp" ::
    
    $ psql odp -Atc "SELECT f_table_name|| '('||type||')' from geometry_columns"
    
    odp(POINT)


optimisation composant openLayers
=================================

construire un OpenLayers.js compresse dans le repertoire build ::

    $ cd buill
    $ python build.py 

le fichier fait 800 ko au lieu de 3 Mo

- compression lite ::

    $ python build.py lite.cfg
    le fichier fait 120 ko
    regarder dans le fichier "lite" les fichiers qui sont inclus
    et éventuellement le compléter
