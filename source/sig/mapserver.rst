.. _mapserver:

#########
Mapserver
#########

mapserver n est pas utilise et il a été préféré qgis_server dans les deploiements openMairie


Ce chapitre propose quelques principes de map server 


Principes
=========


MapServer permet de générer des cartes à partir de données diverses et de fichiers de configuration,
qui contiennent les paramètres décrivant la façon dont les données doivent être présentées, les mapfiles. 


Installation UBUNTU
===================

Verifiez que les depots Universe et Multiverse font partie de vos sources de mise a jour. 
installer les paquets suivants
(obligatoire)

- cgi-mapserver 

- mapserver-bin 

- mapserver-doc


creer un repertoire avec droit d ecriture pour www-data pour stocker les images crees par mapinfo var/www/tmp/ pour ubuntu, ou changer le chemin de ce repertoire dans les openmairie_cimetiere/sig/map/xxxx.map ::
    
        WEB
            IMAGEPATH "/var/www/tmp/" 
            IMAGEURL "/tmp/" 
        END

.. attention:: 
   
   Verifier le chemin dans  openmairie_cimetiere/sig/var.inc qui doit correspondre a votre installation

