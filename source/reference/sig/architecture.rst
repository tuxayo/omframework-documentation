.. _architecture:

========================
la nouvelle architecture
========================

Une classe om_sig_map.class.php a été créé dans le core d'openMairie.

Attention la base de données  om_sig_* a changée dans la version 4.4.5
(voir intégration 4.4.0 vers 4.4.5)

Les appels à la classe om_sig_map dans le core sont faits par :

- tab_sig.php

- form_sig.php

- les scripts spécifiques spg

- le script export_sig.php dans le repertoire  scr

- le script reqmo ... pour appel a un tab_sig et unretour en format geojson

om_map_class appelle les fonctions javascripts contenus dans js/sig.js



tab_sig.php
===========

les requêtes de mises à jour du geom ne sont plus initiée en form


les variables get sont les suivantes ::
    
    obj : carte d'om_sig_map sur laquelle on travaille
    idx : numéro d'enregistrement - peut être vide si on part d'une recherche
    popup -> 0 : le menu est affiché; 1 : il ne l'est pas (tableau/options)
    seli : géometrie selectionnée
    min max -> sert à la prépartion du geojson
    etendue -> tableau options
    reqmo -> tableau options
    premier, recherche, selectioncol, adv_id -> tableau options
    valide -> tableau options
    style -> tableau options
    onglet -> tableau options
    advs_id=
    recherche  -> recherche simple  exemple : 02
    retour     -> exemple : tab
    

    
appel librairie : "../core/om_map.class.php"; ::

    new om_map : obj + options
    om_map = new om_map(obj, options);
    om_map->recupOmSigMap();
    om_map->recupOmSigflux();
    om_map->computeFilters(options['idx']);
    om_map->setParamsExternalBaseLayer();
    om_map->prepareCanevas();


form_sig.php
============

les requêtes de mises à jour du geom ne sont plus initiée en form

variables get ::
    
    obj, idx
    popup -> tableau options
    min max -> sert à la prépartion du geojson
    etendue -> tableau options
    reqmo -> tableau options
    premier, recherche, selectioncol, adv_id -> tableau options
    valide -> tableau options
    style -> tableau options
    onglet -> tableau options
    validation (variable de validation spécifique form)
    
appel librairie : "../core/om_map.class.php"; ::

    New om_map : obj + options
    om_map->recupOmSigMap
    preparation du geojson 
    om_map->prepareForm( min, max, validation, geojson);


les fichiers scr/sig_json.php, sig_pannier.php, sig_wkt.php sont supprimés par rapport à
la version 4.4.x


scr/travail_tab_sig.php ???


spg/map_compute_geom.php
========================

En utilisation du pannier, computegeom fait une union des geometries lorsque l on fait une validation (bouton v)
après selection de multiples géométries

variables get ::
    
    obj, idx
    popup -> tableau options
    min max -> sert à la prépartion du geojson
    etendue -> tableau options
    reqmo -> tableau options
    premier, recherche, selectioncol, adv_id -> tableau options
    valide -> tableau options
    style -> tableau options
    onglet -> tableau options


appel librairie : "../core/om_map.class.php"; ::

    om_map = new om_map(obj, options);
    om_map->recupOmSigMap();
    ...
    echo sep.om_map->getComputeGeom(c, geojson[i]);

spg/map_get_filters.php
========================

gestion du filtre qgis dans om_sig_map_flux

Exemple le père et tous ses fils ::

    SELECT 'fpere_point:²pere² IN ( '||pere||' );fpere_perim:²pere² IN ( '||pere||' );ffils_point:²pere²
    IN ( '||pere||' );ffils_point:²pere² IN ( '||pere||' );ffils_perim:²pere² IN ( '||pere||' )'
    AS buffer FROM ( SELECT array_to_string(array_agg(pere), ' , ') AS pere FROM &DB_PREFIXEpere
    WHERE pere IN (SELECT &idx::integer UNION &lst_idx) ) a


variables get ::
    
    obj, idx
    popup -> tableau options
    min max -> sert à la prépartion du geojson
    etendue -> tableau options
    reqmo -> tableau options
    premier, recherche, selectioncol, adv_id -> tableau options
    valide -> tableau options
    style -> tableau options
    onglet -> tableau options


appel librairie : "../core/om_map.class.php"; ::

    om_map = new om_map(obj, options);
    om_map->recupOmSigMap();
    om_map->recupOmSigflux();
    ...
    om_map->computeFilters(idx_sel);


spg/map_get_geojson_cart.php
============================

Récupére les géométries du pannier

variables get ::
    
    obj, idx
    popup -> tableau options
    min max -> sert à la prépartion du geojson
    etendue -> tableau options
    reqmo -> tableau options
    premier, recherche, selectioncol, adv_id -> tableau options
    valide -> tableau options
    style -> tableau options
    onglet -> tableau options


appel librairie : "../core/om_map.class.php"; ::

    om_map = new om_map(obj, options);
    om_map->recupOmSigMap();
    om_map->recupOmSigflux();

    lst=om_map->getGeoJsonCart(cart, lst);
    ... affichage de la liste

spg/map_get_geojson_markers.php
===============================

Renvoie en format json les marqueurs (ex bulles)

variables get ::
    
    obj, idx
    popup -> tableau options
    min max -> sert à la prépartion du geojson
    etendue -> tableau options
    reqmo -> tableau options
    premier, recherche, selectioncol, adv_id -> tableau options
    valide -> tableau options
    style -> tableau options
    onglet -> tableau options


appel librairie : "../core/om_map.class.php"; ::

    om_map = new om_map(obj, options);
    om_map->recupOmSigMap();
    lst=om_map->getGeoJsonMarkers(options['idx']);
    ... affichage de la liste
    
spg/map_redirection_onglet.php
==============================

Ce programme sert à faire afficher sous form : fenetre dans une fenetre courante

Il appelle utils.class.php
    
Il permet la redirection vers le formulaire de l'objet en visualisation (action=3) si l'objet 
existe et de faire un ajout sinon.

L'ajout ne fonctionne pas.
En cas d'appui sur formulaire, le message apparait  : "aucun enregistrement sélectionné"


scr/export_sig.php
==================

ce programme permet de faire un affichage sur la base d'un tab ou d une recherche.

- export suivant le moteur de recherche (equivalent de export csv)

- image dans app/img + app/css (appel à l image)

- modification de core/om_layout.php
  function display_table_global_action 

- pour l instant ou csv ou sig -> pas de possibilité de faire les 2.

- bug en recherche si aucun enregistrement -> ouvre un nouvel onglet


scr/requeteur.php
=================

La modification de programme permet de lancer un tab_sig depuis  reqmo.

Le programme a donc évolué pour gérer cette possibilité mais elle n est pas encore effective (voir alain)

Par contre une modification est obligatoire dans core/om_layout.class.php pour proposer l'option 'json'



Nouvelles images dans img/ et nouvelle css pour l'interface om_sig
==================================================================

Des nouvelles images pour l interface ::

    map-nav.png
    map-geoloc.png
    map-form.png
    map-edit-valid.png
    map-edit-select.png
    map-edit-record.png
    map-edit-point.png
    map-edit-modif.png
    map-edit-get-cart.png
    map-edit-erase.png
    map-edit-draw-regular.png
    map-edit-draw-polygon.png
    map-edit-draw-line.png
    map-edit.png
    map-distance.png
    map-area.png
    
des nouvelles images pour recherche csv ou sig (geojson) ::

    sig.png
    csv.png


Nouvelle css à mettre en layout_jqueryui_before.css

Probleme d'affichage à regler



    

    
    

    
