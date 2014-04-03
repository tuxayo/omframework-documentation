.. _install:

#########################
installation d'om_sig_map
#########################

Pour faire fonctionner tab_sig.php, il faut :

- installer postgis sous postgres

- openlayers qui est de base dans le framework lib/openlayers



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
