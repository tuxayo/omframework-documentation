.. _principe:


Il est necessaire que l'API openLayers soit dans le framework :

lib/openlayers



#########
Principe
#########


Il est proposé dans ce chapitre de decrire le module
tab_sig.php qui permet la geo localisation d'objet dans openMairie


Ce module est accessible dans la version 4.01 du framework et il est utilisé
dans les applications openMairie suivantes ::

    openmairie domainepublic
    openmairie foncier
    openmairie debitboisson
    openmairie cimetiere
    openmairie circulation
    openmairie taxepub
    openmairie triSelectif
    openmairie adresse postale
    openmairie openelec (projet)
    openmairie resultat (projet)
    openmairie dia (projet)
    openmairie erp (projet)
    

L'objectif de tab_sig_map est de permettre une saisie le plus souvent automatique 
par un point, ligne, multiligne, polygone, multipolygone. Cette saisie est  stockée dans la base métier postgresql.
Elle est affichée sur des fonds existants sur internet : google sat, openStretmap ou bing (pour l instant) en utilisant le composant javascript openLayers

Il n'est donc pas nécessaire de disposer d'un SIG pour utiliser tab_sig.php.

Le format de stockage des données pgsql est celui de l'OGC et il est accessible aux
clients libres où propriétaires qui respectent ce format
(QGIS, GRASS, VEREMAP  ... pour les clients libres)

============================
Géo localisation automatique
============================

L'enjeu est de limiter au maximum la géo localisation manuelle dès
qu'il y a une possibilité de géo localisation automatique.

Elle se fait au travers de 2 programmes (voir paragraphe sur le geocodage):

- adresse_postale.php : positionnement suivant le numero et rue

- adresse_postale_google.php : positionnement suivant le numero et rue avec google

- adresse_postale_bing.php : positionnement suivant le numero et rue avec bing

- adresse_postale_mapquest.php : positionnement suivant le numero et rue avec mapquest


La géolocalisation automatique peut se faire sur une base externe
postgresql (eventuellement via une vue)

le script tab_sig.php permet de saisir manuellement le point.



==================
Affichage de carte
==================

L'affichage se fait avec openLayers dont le composant est de base
dans le framework openMairie : lib/openLayers. (le composant est
installé de manière a être optimisé avec une css openmairie)

La librairie proj4 inclus dans lib/openLayers permet de pouvoir utiliser
les projections lambert sud et lambert 93.

La projection géographique et Mercator est de base dans openLayers

L'enjeu est donc de projeter les données stockées dans la base "métier"
postgresql - postgis (les communes devant utiliser le lambert93) en mercator
pour être lisible avec les cartes accessibles sur internet.

L'affichage des datas est fait au travers d'une requête postgresql
qui alimente un tableau json lu comme une couche openLayers.

La data à modifier est fourni par requete postgresql au format wkt à openLayers.
(voir paragraphe layers)

tab_sig.php permet ::

    - l affichage de/des  fond(s)
    - l'affichage de données (data)
    - l affichage du geométries qui peut être créé ou déplacé (couche wkt)

dans la version 4.2.0, tab_sig permet aussi ::

    - l'affichage de flux wms et wfs (getmap) et de recuperer les données (getfeature)
    - la collation de géométrie dans un pannier et son enregistrement en multi géométries




=======================
Paramétrage de la carte
=======================

Le paramétrage général (contenu dans scr/tab_sig.php) des cartes  est modifiable dans 
dyn/var_sig.inc ::

    // *** parametre de tab_sig.php ***
    // generer une cle pour le site : http://code.google.com/intl/fr/apis/maps/signup.html
    $cle_google = "";
    $fichier_jsons="json_points.php?obj=";
    $fichier_wkt="wkt_point.php";
    //zoom par couche : zoom standard permettant un passage de zoom a l autre
    $zoom_osm_maj=18;
    $zoom_osm=14;
    $zoom_sat_maj=8;
    $zoom_sat=4;
    $zoom_bing_maj=8;
    $zoom_bing=4;
    // popup data contenuHTML
    $width_popup=200;
    $cadre_popup=1;
    $couleurcadre_popup="black";
    $fontsize_popup=12;
    $couleurtitre_popup="black";
    $weightitre_popup="bold";
    $fond_popup="yellow";
    $opacity_popup="0.7";
    // image localisation maj ou consultation
    $img_maj="img/punaise.png";
    $img_maj_hover="img/punaise_hover.png";
    $img_consult="img/punaise_point.png";
    $img_consult_hover="img/punaise_point_hover.png";
    $img_w=14;
    $img_h=32;
    $img_click="1.3";// multiplication hauteur et largeur image cliquee
    
    // *** parametres d om_sig_map.class.php, om_sig_wms.class.php
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
        



