.. _numerotation:

######################################################################
Convention de numérotation des versions des applications et librairies
######################################################################

Il est convenu de numéroter les versions sur 3 chiffres séparé par des points.

exemple: openMairie 4.0.0


Le premier chiffre représente une version majeure

Le deuxième chiffre est une évolution mineure

Le troisième chiffre est une correction de bug

Les versions beta sont indiqués en fin de numérotation et ne sont jamais maintenues

    openmairie_exemple_4.0.0beta

    

Seule la dernière version opérationnelle est maintenu


##########################
Passage à la version 4.2.0
##########################

La version 4.2.0 du framework prend en charge plus de fonctionnalités et donne toutes possibilité de surcharge aux applications

- surcharge des objets généres par le generateur 

- surcharge des composants de base openMairie stocké dans core

- surcharge de la présentation de base (dans img et css), des thèmes (om-theme) dans app/css app/img

- surcharge du javascript de base app/js/script.js


EXTERNALS.txt
=============

vider les 10 repertoires concernés avant de lancer externals

core est le repertoire contenant la librairie openMairie (4.2.0)

om_theme est le theme de l application

appliquer extermnals openmairie /EXTERNALS.txt ::

    core  svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/core/
    spg   svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/spg/
    scr   svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/scr/
    lib   svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/lib/
    css   svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/css/
    js    svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/js/
    img   svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/img/
    pdf   svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/pdf/
    php   svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/php/
    
    om-theme svn://scm.adullact.net/svnroot/openmairie/externals/om-theme/kinosura/tags/1.0.0



Ce qui est spécifique à l'application ::

    app     scripts spécifiques de l'application sont à mettre en app
            noter dans specific.txt les spécificités
    data    scripts sql d'initialisation de la base de données
    dyn     paramétrage de l application
    gen     générateur des objets de l application
    obj     surcharge des objets de l application générés
            surcharge du core "openmairie" om_dbformdyn , om_formulaire (4.2.0), om_application (util) 
    sql     requête sql en surcharge de gen/sql
    tmp     fichiers temporaires de l application (mettre les droits d'écriture pour apache)
    trs     fichiers uploadés par l application (mettre les droits d'écriture pour apache


regenerer les tables avec genfull.php
=====================================
mettre les droits  d ecriture a www-data dans gen : $ sudo chmod-R 777
sur les nouvelles tables gerer par www-data remettre les droits d ecriture
$ sudo chmod -R 777


Evolution om_sig_point vers om_sig_map
======================================

om_sig_map est le nouvel outil sig d openMairie


ne concerne que pgsql

executer le script data/pgsql/ver4.2.0.sql
