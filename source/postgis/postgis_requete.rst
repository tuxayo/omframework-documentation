.. _postgis_requete:



Ce chapitre propose quelques requetes utilisant les fonctions postgis



surface :

    SELECT Area2d(geom) FROM cimetiere;

perimetre :

    SELECT perimeter(geom) FROM cimetiere;

Les	départements limitrophes du Tarn	 
  
          SELECT d.nom_dept 
          FROM departement as d, 
          (SELECT the_geom FROM departement WHERE 
          departement.code_dept = '81') as tarn 
          WHERE ST_Touches(d.the_geom, tarn.the_geom)
          
Les départements à moins de 200km de la limite du Tarn :

          SELECT DISTINCT d.code_dept, d.nom_dept 
          FROM departement as d, 
          (SELECT the_geom FROM departement WHERE 
          departement.code_dept = '81') as tarn, 
          ST_buffer(tarn.the_geom, 200000) as le_buffer 
          WHERE ST_contains(le_buffer, d.the_geom) 

Les cours d'eau du Tarn : 
          SELECT DISTINCT ce.toponyme 
          FROM cours_eau as ce, 
          (SELECT the_geom FROM departement WHERE code_dept = '81') as tarn 
          WHERE ST_intersects(tarn.the_geom, ce.the_geom) 

Les points de mesure hydrologiques du Tarn : 

          SELECT DISTINCT mes.nom_usuel 
          FROM st_eausup_ag as mes, 
          (SELECT the_geom FROM departement WHERE code_dept = '81') as tarn 
          WHERE ST_Contains(tarn.the_geom, mes.the_geom) 

Le nom des points de mesure a proximité de la Garonne : 
                                      7 

Création d’une table temporaire pour stocker un buffer de 100m autour de la Garonne : 
   create table bgaronne as 
   select st_buffer(the_geom, 100) as the_geom 
   from cours 
   where toponyme = 'La Garonne'
   
Recherche des points de mesure dans ce polygone : 

   select st_eausup_ag.nom_usuel 
   from st_eausup_ag, bgaronne 
   where st_contains(bgaronne.the_geom, st_eausup_ag.the_geom) 
