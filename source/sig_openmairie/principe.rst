.. _principe:

#########
principe
#########


Il est proposé dans ce chapitre de decrire le module
om_sig_point qui permet la geo localisation d'objet dan dans openMairie
avec un  point


Ce module est accessible dans la version 4.01 du framework et il est utilisé
dans les applications openMairie suivantes ::

    openmairie domainepublic
    openmairie foncier
    openmairie debitboisson
    openmairie erp
    openmairie cimetiere

L'objectif d'om_sig_point  est de fournir une géo localisation  automatique ou manuelle
par un point stocké dans la base métier postgresql sur des fonds existants sur internet :
google sat, openStretmap ou bing (pour l instant) en utilisant le composant javascript openLayers

Il n'est donc pas nécessaire de disposer d'un SIG pour utiliser OM SIG.

Le format de stockage des données pgsql est celui de l'OGC et il est accessible aux
clients libres où propriétaire qui respectent ce format
(QGIS, GRASS, VEREMAP  ... pour les clients libres)

============================
géo localisation automatique
============================

L'enjeu est de limiter au maximum la géo localisation manuelle dès
qu'il y a possibilité de géo localisation automatique.

Elle se fait au travers de 2 programmes :

- adresse_postale.php : positionnement suivant le numero et rue

- centroid_parcelle.php : positionnement suivant calcul du centre de la parcelle

La géolocalisation automatique peut se faire sur une base externe
postgresql suivant un systeme de bus de données

le script om_sig permet de saisir manuellement le point.


==================
affichage de carte
==================

L'affichage se fait avec openLayers dont le composant est de base
dans le framework openMairie : lib/openLayers. (le composant est
installé de manière a être optimisé et il est utilisé une css openmairie)

La librairie proj4 inclus dans lib/openLayers permet de pouvoir utiliser
les projections lambert sud et lambert 93.

La projection géographique et Mercator est de base dans openLayers

L'enjeu est donc de projeter les données stockées dans la base "métier"
postgresql - postgis (les communes devant utiliser le lambert93) en mercator
pour être lisible avec les cartes accessibles sur internet en mercator

L'affichage des datas est fait au travers d'une requête postgresql
qui alimente un tableau json lu comme une couche openLayers.

La data à modifier est fourni par requete postgresql au format wkt à openLayers.
(voir paragraphe layers)

=======================
paramétrage de la carte
=======================

Le paramétrage général des cartes  se fait dans :

sig/var_sig_point ::

    // objet par defaut
    $obj='odp';
    
    //zoom par couche : zoom standard permettant un passage de zoom a l autre
    $zoom_osm_maj=18;
    $zoom_osm=14;
    $zoom_sat_maj=8;
    $zoom_sat=4;
    $zoom_bing_maj=8;
    $zoom_bing=4;
    
    // parametrage du popup
    $width_popup=200;
    $cadre_popup=1;
    $couleurcadre_popup="black";
    $fontsize_popup=12;
    $couleurtitre_popup="black";
    $weightitre_popup="bold";
    $fond_popup="yellow";
    $opacity_popup="0.7";
    // parametrage des images localisation maj ou consultation
    $img_maj="img/punaise.png";
    $img_maj_hover="img/punaise_hover.png";
    $img_consult="img/punaise_point.png";
    $img_consult_hover="img/punaise_point_hover.png";
    $img_w=14;
    $img_h=32;
    $img_click="1.3";// multiplication hauteur et largeur image cliquee
    
    // parametrage des etendues et espg pour om_point_sig.class.php
    $contenu_etendue[0]= array('4.5868,43.6518,4.6738,43.7018',
                              '4.701,43.3966,4.7636,43.4298',
                              '4.71417,43.64,4.72994,43.65166',
                              '4.72345,43.55348,4.73134,43.55932',
                              '5.2094,43.4136,5.3345,43.4759'
                              );
    $contenu_etendue[1]= array('agglomeration',
                              'salin de giraud',
                              'raphele',
                              'Mas thibert',
                              'vitrolles'
                              );
    $contenu_epsg[0] = array("","EPSG:2154","EPSG:27563");
    $contenu_epsg[1] = array("choisir la projection",'lambert93','lambertSud');
    

Le paramétrage particulier d'une carte se fait avec l'objet métier
om_point_sig.class.php accessible dans le menu administration -> OM SIG

Il est possible de copier une carte et de paramétrer  les champs suivants::

    - id : identifiant unique (obligatoire)
    - libelle
    - fonds a afficher et data
    - étendue et epsg (voir sig/var_sig_point.inc)
    - url (qui pointe sur la fiche ou le formulaire de saisie)
    - requete sql qui affiche les données json et qui doit désigné :
        le titre
        la description
        l idx
    - la mise a jour si oui, le champ géometrique et la table maj
    - le retour de la carte

Ces cartes sont possibles d'intégrer dans des menus, dans un formulaire tab
(si mise a jour) ou dans le tableau de bord (voir widget)

Dans le lien, il est possible de définir ::

- la  carte a afficher suivant l'id : ?obj=   Obligatoire
- le fond affichable par défaut : sat, bing, osm : &fond =
- l'étendue : &etendue =
- l enregistrement à modifier : &idx=

