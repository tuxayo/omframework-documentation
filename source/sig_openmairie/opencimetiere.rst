.. _opencimetiere:

###################
objet opencimetiere
###################


ce chapitre propose de décrire le SIG implementé dans la version 2.02
d'openCimetiere. 

Ce module est opérationnel avec la version 2.02 d'openCimetiere.

Il necessite :

- le module lib/openLayers

- map server installé sur le serveur (voir mapserver)


===============
Les scripts sig
===============

Dans le répertoire sig, il y a 3 scripts :

    - tab_sig.php : affiche la carte suivant les parametres de nom_objet.sig.inc
    
    - form_sig.php : modifie le champ geometry de l'objet
    
    - delete_sig.php : supprime le contenu  du champ geometry


Il y a un repertoire map ou sont stockés les fichiers map utilisés par mapserver

Le fichier var.inc stocke les variables suivantes ::
    
    // chemin de la carte pour mapserver
    $path_map="http://localhost/cgi-bin/mapserv?map=/var/www/
                      openmairie_cimetiere/sig/map/";
    
    // polygon créé par défaut
    $creation_polygon="POLYGON((0 0,0 0))";

Le fichier style.css contient la css de l interface

Le lancement du sig se fait avec tab_sig.php sous la forme suivante :

/sig/tab_sig?idx=29&obj=emplacement

où :

$idx est l' enregistrement ou modifier/ajouter/supprimer la geometry  dans le champ geom

$obj est la table ou la geometry doit etre modifiée : cimetiere, emplacement, voie, zone

ATTENTION, il ne peut etre saisi qu un enregistrement existant dans la table


Dans pgsql, il y a les parametres des affichages tab_sig.php

En effet, il s agit d'afficher pour chaque objet la partie (box) de la carte à afficher et
l'objet courant et le titre


exemple : objet concession : 

L'objectif dans openCimetiere est de saisir d'un polygone geometrique
representant un emplacement en fonction de la zone

La procedure est la suivante :

-> afficher une partie de la carte du cimetière (la zone de l'emplacement)

-> dessiner un polygone représentant la géometrie de la concession saisie

-> enregistrer la geometry (wkt) : form_sig.php ou supprimer : delete_sig


les paramétres sig sont dans sql/pgsql/emplacement.sig.inc::
    
    $fichier_map ="cimetiere.map";                      nom de la carte dans /map
    $titre= "Emplacement ".$idx." - ";                  titre affiché
    $texte="";                                          texte affiché
    $aff="zone";                                        fond de carte
    $class_map="bigmap";                                taille : smallmap pu bigmap
    $sql_idgeom="select voie.zone from emplacement " .  Identifiant a afficher
        "inner join voie on emplacement.voie=voie.voie " .
            "inner join zone on zone.zone=voie.zone
            where emplacement.emplacement =".$idx;
    // box et zone
    $affiche='famille';                                 champ affiché sur  emplacement 
    $sql ="select ('cimetiere ' ||cimetierelib||'       selection du libelle et de la box
            zone '|| zonelib) as lib,box(zone.geom)
            as box  from zone inner join cimetiere
            on cimetiere.cimetiere=zone.cimetiere ";


============
Installation
============

Il est necessaire d(installer sur le serveur apache, mapserver.

Il faut aussi installer la cartouche postgis avec postgres

Les installations sous UBUNTU sont les suivantes ::

    * installer les paquets postgis 
    - postgis 
    - postgresql-8.3.postgis 
    
    * installer mapserver 
    Verifiez que les depots Universe et Multiverse font partie de vos sources de mise a jour. 
    installer les paquets suivants
    (obligatoire)
    - cgi-mapserver 
    - mapserver-bin 
    - mapserver-doc 
    (optionnel)
    - php5-mapscript. 
    relancer apache  $  /etc/init.d/apache2 restart 
    
    *** Installation de la base opencimetiere avec postgis
    
    * creer la base opencimetiere (si elle n'est pas deja creee)
    * create language "plpgsql" 
    * executer (version postgis <1.5) la requete lwpostgis.sql -> fonction postgis
      ou executer (version postgis >= 1.5) la requete /usr/share/postgresql/8.3/
                                                    contrib/postgis-1.5/postgis.sql 
    * executer spatial_ref_sys .sql qui remplit la table de donnees spatial_ref_sys 
    * VERIFICATION : les tables suivantes sont presentes :
        * table geometry_columns : index des geometries (vide) 
        * table spation_ref_sys : liste des references spatiales (3162 lignes environ)
    * executer les scripts d'initialisation de la base opencimetiere
        * data/pgsql/init.sql
        * data/pgsql/initsig.sql
        * data/pgsql/initsig_data.sql (optionnel) jeu de donnees
