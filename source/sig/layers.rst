.. _layers:

############
Objet layers
############


ce chapitre propose de décrire l'utilisation de l'objet layers
d'openLayers dans tab_sig.php.




Dans tab_sig.php, il y a 3 types de layers :

- les fonds de cartes existants sur internet (base layers)
 
- les données issus de postgresql (overlays)

- les données wms (overlay)



Les fonds
=========

Il est proposé les fonds suivants :

osm : openstreetmap

sat : satelite google 

bing : satellite microsoft 


Les datas
=========

Information de la carte : layer_info 

Cette couche fait appel à sig_json.php

Il est possible de faire appel a un autre script (voir dyn/var_sig.inc)

La requête pgsql est paramétrée dans la table om_sig_point et doit définir les champs
geom, titre, description et texte.

sig_json.php présente tous les enregistrements d'un même
point (même géom) sur un  seul popup

En effet, il est constitué un popup lorsque l on clique sur l objet
et donne la possibilité à un accès URL parametrée dans om_sig_point::


Les flux wms
============

Le paramètrage des flux dans om_sig_wms


L'affectation des flux dans une carte dans om_sig_map_wms


La notion de pannier
====================

scr/sig_pannier.php




La géométrie à modifier : couche vectors :
==========================================

Le chargement de la couche vectors se fait si dans la table om_sig_map,
la case maj est activé 

La géométrie est récupérée par le script sig_wkt.php (appel a un script parametrable dans var_sig.inc)
et la carte est centrée sur la géométrue::

Il est possible de :
    
    - positionner manellement la géométrie
    - déplacer la géométrie
    - enregistrer la géometrue  : selectionner la géométrie, le programme
        form_sig.php est chargé en fenetre et permet de supprimer
        la géométrie (champ geometrique = null)  ou modifier cette géométrie.
    
    Les fonctions javascript et les controles sont activées suivant chaque état.
   
Dans dyn/form_sig_update.inc.php, il est possible de paramétrer des post traitements de saisie

Dans dyn/form_sig_delete.inc.php, il est possible de paramétrer des post traitements de suppression


Les géométries complémentaires
==============================

Il peut y avoir plusieurs géométries pour un même objet.

Elles sont saisies dans om_sig_map_comp.class.php



   