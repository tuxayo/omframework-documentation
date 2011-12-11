.. _data_sig:

########
data_sig
########


ce chapitre propose de décrire la récupération de données SIG nécessaires à la mise en oeuvre
des scripts SIG internes openMairie.



Saisir le périmètre de sa commune
=================================

Il s'agit d'adapter ses cartes au périmètre de sa commune

Aller sur openstreetMap  http://www.openstreetmap.org/

Chercher la ville : exemple "Gréasque"

Ajuster la carte aux frontières communales

Aller dans l onglet export et noter les coordonnées géographiques "zone à exporter"

.. image:: ../_static/osm_export.png 

Dans le fichier dyn/var_sig.inc, modifier le tableau de variables avec les coordonnées
de la manière suivante ::

    $contenu_etendue[0]= array('5.5155,43.4081,5.5781,43.4426');
    $contenu_etendue[1]= array('greasque'); 

Modifier les cartes de om_sig_point


Récupérer les données de l'IGN
==============================

Exemple de récupération de données "parcelle" (fichier shape du CRIGE PACA)

Les données de l'IGN sont fournies aux communes par département.

Insérer le fichier parcelle dans la base (exemple IGN) ::

    shp2pgsql -s 2154 -I -D -W LATIN1 PARCELLE.SHP | psql ign

Il est créer dans la base "ign" une table parcelle décrite ci dessous ::

        gid serial NOT NULL,
		numero character varying(4),
		feuille smallint,
		section character varying(2),
		code_dep character varying(2),
		nom_com character varying(45),
		code_com character varying(3),
		com_abs character varying(3),
		code_arr character varying(3),
		the_geom geometry

ainsi qu un enregistrement parcelle dans géometry_columns (postgis est obligatoire
dans la base) et un index

Selectionner les parcelles de la commune concernée et inserer les dans une nouvelle table ::

    insert into parcelle_greasque 	(parcelle, section, commune, geom)
    select 	section||numero, section, code_dep||code_com, the_geom 	from parcelle
        where code_com = '046';

Pour mettre à jour le champ surface de parcelle dans openfoncier ::

    update parcelle set surface = round(cast(area2d(geom) as numeric), 2)
    

Recupération des données de la DGI
==================================

Les fichiers textes de la DGI sont dans un format  récupéré dans le cadre
de l application openCadastre qui reconstitue des tables postgresql.

Les fichiers gémétriques au format EDIGEO ne sont pas récupérés compte tenu de son format non standard
et il est préféré utiliser les formats shape de l IGN


Recuperation de la base adresse de l'IGN
========================================

