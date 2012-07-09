.. _qgis_server:

###########
qgis_server
###########

Pour gérer les flux wms et wfs, il a été implémenté qgis_server

Ce chapitre propose quelques principes d'utilisation de qgis server



Installation de QGIS server sur UBUNTU
======================================

Article d'origine
http://geotribu.net/node/286
modifié suite teste le 07/06/2012 avec la version 1.8 de qgis server

Installation de qgis server

La communication entre QGIS Mapserver et notre serveur Web s'appuie sur le protocole CGI/FCGI :: 

    $ sudo apt-get install libfcgi-dev

l'installation de QGIS Mapserver se fait en utilisant le dépôt ubuntugis ::

    $ sudo apt-get install qgis-mapserver

Il est créé un répertoire /usr/lib/cgi-bin/, le fichier qgis_mapserv.fcgi qui interprete les requêtes WMS et retourne ensuite sous forme d'imagettes (flux wms). 


parametrage de qgis server
==========================

il faut créer un nouveau dossier dans ce répertoire cgi-bin pour chaque projet qgis (si on veut un acces externe hors poste de production localhost). 

exemple pour le projet formation ::

    usr/lib/cgi-bin$ sudo mkdir formation 

Ensuite,  il faut créer trois liens symboliques. 

Le premier pointant vers script qgis_mapserv.fcgi, le second vers votre projet QGIS et enfin le dernier vers le fichier wms_metadata.xml. 
On peut personnaliser ce dernier fichier en y ajoutant les différentes informations concernant le producteur de la donnée (nom de l'organisme, nom du référent...).

Ceci permet d'acceder en wms avec openlayers avec une IP externe :: 

    /usr/lib/cgi-bin/formation$ sudo ln -s /var/www/projet/formation/qgis/formation.qgs .

pour l'acces externe, ces deux liens ne servent pas   mais ils sont importants pour la fonction "getCapabilities" ::

    /usr/lib/cgi-bin/formation$ sudo ln -s ../qgis_mapserv.fcgi .
    /usr/lib/cgi-bin/formation$ sudo ln -s ../wms_metadata.xml .


requetes WMS / WFS
==================

tab_sig.php utilise dans les requêtes ::

    getMap
    getFeature

