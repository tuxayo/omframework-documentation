.. _postgis:

#######
postgis
#######

ce chapitre propose de décrire les possibilités d'utilisation de postgis dans openMairie.
Il n'est pas bien sûr exhaustif et se completera au fur à mesure de notre expérience
dans les applications openMairie.


principes
=========


PostGIS ajoute à postgresql ::

    - ses types de données
    - ses fonctions,  
    - deux tables utilitaires : geometry_colums et spatial_ref_sys.
        geometry_colums sert à indiquer au logiciel quels sont les champs contenant des types
        géographiques dans chacune des tables.
        spatial_ref_sys contient les paramètres des systèmes de projection  supportés
        et sert en interne au logiciel. 


Cf. : http://postgis.refractions.net/documentation/manual-1.3/ch06.html 


base et schéma
==============

Il est noté que les applications openMairie peuvent s'installer dans un schéma.

Les tables et fonctions postgis sont alors accessible dans le schéma public.