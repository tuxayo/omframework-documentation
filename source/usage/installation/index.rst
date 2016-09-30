.. _installation:

############
Installation
############


Installation de qgis serveur sur linux debian et ubuntu
=======================================================

Commandes d'installation ::

  apt-get install apache2 libapache2-mod-fcgid qgis-mapserver
  a2enmod cgid
  service apache2 restart

Test de bon fonctionnement ::

  Exemples d'URLs pour tester le bon fonctionnement du serveur :
  http://XX.XX.XX.XX/cgi-bin/qgis_mapserv.fcgi?SERVICE=WMS&VERSION=1.3.0&REQUEST=GetCapabilities

Liens de références ::

  http://adrien.caillot.free.fr/?p=9395
  http://hub.qgis.org/projects/quantum-gis/wiki/QGIS_Server_Tutorial
  http://io.gchatelier.fr/blog/installation-qgis-server-publication-dun-projet-qgis/

Crédit : Jean Christophe BECQUET APITUX

Niveau de version nécessaire au fonctionnement de la version om 4.5.0
=====================================================================

Dans le nouveau fonctionnement des éditions, il est nécessaire d'avoir la libraire libxml au niveau  2.9.0. pour ne pas rencontrer des problèmes d encodage.

Les distributions suivantes fonctionnent avec cette librairie ::

  ubuntu 14.04 et +
  wamp server 3.06 +
  debian ???
  
Les versions de distributions suivantes n'ont pas la librairie au niveau exigé ::

  ubuntu 12.04 et -
  wamp server 2.0 et -
  WAPP de Bitnami ???
  CentOS ???

Crédit : Laurent GROLEAU Mairie de Marseille

