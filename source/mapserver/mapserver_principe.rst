.. _mapserver_principe:



Ce chapitre propose les principes de map server.


principes
=========


MapServer permet de générer des cartes à partir de données diverses et de fichiers de configuration,
qui contiennent les paramètres décrivant la façon dont les données doivent être présentées, les mapfiles. 

Le plus souvent, MapServer est utilisé sur un serveur Internet pour générer des images dans des pages 
web, et ainsi permettre l'affichage mais aussi la modification, d'images cartographiques sur un site 
Internet. 

On peut aussi utiliser MapServer sur son ordinateur pour générer des cartes, effectuer des 
analyses thématiques, des croisements, etc. 

MapServer existe sous deux formes principales : un exécutable, à utiliser à la ligne de commande 
ou en programme CGI (c'est à dire accédé à distance en mode protégé au travers d'un serveur http 
comme Apache), et une bibliothèque de fonctions PHP, Perl, Ruby, Python, C# ou Java : MapScript. 

Organisation concrète d'un serveur SIG basé sur des composants OpenSource autour de MapServer 


Installation UBUNTU
===================

Verifiez que les depots Universe et Multiverse font partie de vos sources de mise a jour. 
installer les paquets suivants
(obligatoire)

- cgi-mapserver 

- mapserver-bin 

- mapserver-doc


*** creer un repertoire avec droit d ecriture pour www.data pour stocker les images crees par mapinfo
    var/www/tmp/ pour ubuntu
    ou changer le chemin de ce repertoire dans les openmairie_cimetiere/sig/map/*.map ::
    
        WEB
            IMAGEPATH "/var/www/tmp/" 
            IMAGEURL "/tmp/" 
        END

- ATTENTION Verifier le chemin dans  openmairie_cimetiere/sig/var.inc qui doit correspondre a 
votre installation




