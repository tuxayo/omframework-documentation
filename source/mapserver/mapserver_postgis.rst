.. _mapserver_postgis:

####################
mapserver et postgis
####################

Cette page presente les differentes manieres de formaliser la requete
postgresql/postgis

data du layer

CONNECTIONTYPE postgis 
CONNECTION "user=nom_utilisateur dbname=nom_bdd host=serveur" 
DATA "colonne_géométrique FROM table_ou_requête_SQL" 

======
filter
======

Le paramètre FILTER peut contenir une expression de requête SQL
(texte venant après un élément SQL WHERE). 

    DATA "the_geom FROM communes USING UNIQUE gid USING srid=27572"
    FILTER "nom = 'TOULOUSE'"
    
    DATA "the_geom FROM communes USING UNIQUE gid USING srid=27572"
    FILTER "nom = 'TOULOUSE' OR nom = 'BLAGNAC'"
    
    DATA "the_geom from communes USING UNIQUE gid USING srid=27572"
    FILTER "rugby > 1"


======
buffer
======

    DATA "st_buffer FROM (SELECT gid, st_buffer(communes.the_geom, 20000)
          FROM communes WHERE nom = 'TOULOUSE') AS foo USING UNIQUE
          gid USING srid=27572"

========
distance
========

    DATA "the_geom FROM (select gid, the_geom FROM communes
        WHERE Distance((select the_geom from communes
        where nom = 'TOULOUSE'), communes.the_geom) < 20)
        AS foo USING UNIQUE gid USING SRID=27572"

============
intersection
============

    DATA "st_intersection FROM (SELECT tlse.gid, st_Intersection(tlse.the_geom, blagnac.the_geom)
        FROM (SELECT * FROM communes WHERE nom = 'TOULOUSE') AS tlse,
        (SELECT * FROM communes WHERE nom = 'BLAGNAC') AS blagnac)
        AS foo USING UNIQUE gid USING srid=27572"

======
touche
======

    DATA "the_geom FROM (select gid, the_geom FROM communes
        WHERE Touches((select the_geom from communes where nom = 'TOULOUSE'),
        communes.the_geom)) AS foo USING UNIQUE gid USING SRID=27572"