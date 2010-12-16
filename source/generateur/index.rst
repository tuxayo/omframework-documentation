.. _regles:

#############
Le générateur
#############

=============================================================================
construire une application sur la base de l'analyse des informations  du SGBD
=============================================================================

Les informations récupérées dans le SGBD sont les suivantes

 		la liste des tables de la base de données

 		les tables : nom, type , et longueur de chaque champs

Le générateur construit sur cette base le modèle de données sur les principes suivants:

- le nom de la clé primaire est le nom de la table et c'est le premier champ 

- la clé secondaire est le nom de la table en lien 

- si la clé est numérique, elle est automatique. 

- avec la multicollectivité, la création d'un champ « om_collectivite » met en place les accès multicollectivités d'openMairie 4

La version 4.00 ne reprend pas (par rapport a la version 3): 

- l'état reprenant l'enregistrement et les sous états rattachés  à l'état principal par la ou les clés secondaires

- la documentation avec un lien sur les pages des tables reliées par la clé secondaire et l'option dans la documentation globale

- l'option dans le menu et le tableau de bord  

- la prise en compte dans la recherche globale

==============
les assistants
==============

Il est fourni avec le générateur un assistant pour faire les états et les sous états.

Le générateur dans sa nouvelle version est conforme aux principes adoptés dans openMairie 4.00 qui se simplifie en s'enrichissant des composants standards :

la documentation des applications n'est plus automatique. Il s'agit d'utiliser un lien existant ou sera stocké votre documentation (site internet, wiki de l'ADULLACT ...).

la traduction utilise le standard gettext et un fichier openmairie.po et openmairie.mo (fichier compilé avec poedit) dans le repertoire LOCALES

la présentation est faite avec le standart jquery et la présentation spécifique openMairie est abandonnée

openMairie est multi collectivité : les états et les sous états sont générés dans la base de données et peuvent être associé a une collectivité.

Le générateur gére la multicollectivité si un champ « om_collectivité » est créé.

Les schemas et prefixe sont gérés

==========================
les acquis de l'experience
==========================

Cette nouvelle version profite de l'expérience de la précédente :

il s'agit de limiter le nombre de requêtes et d'état générés automatiquement et qui donne peu de lisibilité au fonctionnement de l'application. L'utilisateur a le choix de la requête dans le générateur et choisi les états, sous états à générer avec les assistants.

il s'agit de donner une plus grande souplesse d'utilisation : les fichiers .inc et .form.inc de variables sont créés en répertoire gen/basededonnées. Les variables peuvent être « surchargés » ou customiser dans le répertoire sql/basededonnées

les menus et tableaux de bord ne sont pas générés de manière automatique qui va à l'encontre d'une réflexion sur la présentation ergonomique de ces outils. Le menu est fait avec jquery et la modification n'est plus générée. Il faut modifier manuellement le fichier dyn/menu.inc.php. Pour le tableau de bord, Il faut modifier manuellement le fichier dyn/tbd.inc

===========
le menu gen
===========

.. toctree::
 
    ecran.rst
    analyse_base.rst
    fichier_genere.rst
    parametrage.rst   
