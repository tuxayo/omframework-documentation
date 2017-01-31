.. _integration:

###########
Intégration
###########


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

