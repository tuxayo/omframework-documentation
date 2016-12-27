.. _om_sig_flux:




=================
Saisie des flux :
=================

Les flux permettent de créer des couches internes ou externes à l'application.
Il est traité les flux wms (web map service) et les flux de tuiles (tiles)


Il est possible de lister les flux dans le menu  administration -> option om_sig_flux

.. image:: tab_om_sig_flux.png


Formulaire
==========

Il est possible de modifier / supprimer les flux dans le formulaire de saisie om_sig_flux
en appuyant sur modifier ou supprimer

.. image:: form_om_sig_flux.png


Description des champs :
========================

- om_sig_flux est la clé primaire automatique

- id est l'identifiant unique du flux

- attribution : permet d'afficher sur la carte l'attribution des données(copyright ou copyleft)

- type de flux ::

    wms (vide) : exemple qgis
    tilecache (TCF): tuiles
    sleepy map tile (SMT)
    impression (IMP): envoi d'un getPrint ?
    
- URL : url du flux 

- Couches: saisir les couches séparées par des virgules

- Paramètres varie suivant le type de flux ::

    si c'est un flux wms = vide
    si c'est une impression
        largeur carte dans composeur x 2 :
        hauteur carte dans composeur x 2
    si c'est des tuiles (TCF ou SMT)
        URL pour GetFeatureInfo :
        couches pour GetFeatureInfo :

Exemple d'url de flux wms (sur linux avec qgis) ::

    http://localhost/cgi-bin/qgis_mapserv.fcgi
    ?SERVICE=WMS&VERSION=1.3.0
    &map=/var/www/openfoncier/trunk/app/qgis/openfoncier.qgs


Exemple d'une requete impression : getprint

Le getprint ne fonctionne que pour qgis.

https://hub.qgis.org/wiki/17/QGIS_Server_Tutorial

.. image:: getPrint.jpg



