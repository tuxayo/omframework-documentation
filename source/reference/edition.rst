.. _edition:

################
Module 'Édition'
################


.. warning::

   Cette rubrique est en cours de rédaction.


Le framework openMairie permet d'effectuer des éditions au format PDF. Ce module est composé de trois éléments fonctionnels :

* les listings,
* les étiquettes,
* les états et lettres types.


============
Les listings
============

Description de la fonctionnalité
--------------------------------




Le fichier de paramétrage ``../sql/pgsql/<OBJ>.pdf.inc.php``
------------------------------------------------------------

Un état PDF peut être généré par le générateur (option).

Les paramètres sont les suivants ::

    texte et caractéristique du Titre
    Entête de tableau (nom de colonne)
    caractéristique du tableau
    caractéristique des cellules
    total, moyenne, nombre
    requête SQL



==============
Les étiquettes
==============

Description de la fonctionnalité
--------------------------------



Le fichier de paramétrage ``../sql/pgsql/<OBJ>.pdfetiquette.inc.php``
---------------------------------------------------------------------



==========================
Les états et lettres types
==========================

Les différents types d'éditions sont les suivants 
sont paramétrées depuis l'interface du logiciel. Pour chaque édition PDF des
champs de fusion permettent de récupérer dynamiquement les données à imprimer.

Les éditions sont accessibles dans le menu par ::

    - parametrage -> etat 
    - parametrage -> sousetat
    - parametrage -> lettretype

Depuis la version 4 d'openMairie, les éditions sont conservées dans 3 tables ::

    - om_etat : pour les états  
    - om_sousetat : pour les sous-états
    - om_lettretype : pour les lettres types



Actif, non actif
----------------

Les sous-etats sont liés a un ou plusieurs état.

Les états, sous-états, et lettre type peuvent être "actif" ou "non-actif".

Par défaut sont pris en compte :

1 - l'édition  "actif" de la collectivité

2 - l'édition "actif" de la multicollectivité

3 - l'édition "non-actif" de la multicollectivité


Les éditions d'une collectivité ayant le statut "non-actif" ne sont pas prises
en compte.


Paramétrer des états
--------------------

Il est conseillé d'utiliser l'assistant état du générateur.

Les paramètres sont les suivants ::

    orientation portrait ou paysage
    format="A4", A3
    position et nom  du logo 
    titre de l état
    position et caractéristiques du titre
    corps de l état
    position et caractéristiques du corps
    la requête SQL
    les sous-états associés et les caractéristiques


Pour le corps et le titre, les zones entre crochets (exemple [nom]) sont les
champs de fusion, sélectionnés par la requête SQL. 

Ces champs de fusion peuvent être mis en majucule ou en minuscule selon les 
besoins grâce aux balises <MAJ></MAJ> et  <min></min>.

EX. :

Requête SQL = SELECT nom as nom FROM A;

Cette phrase dans un PDF 
'Lorem [nom] dolor sit amet, <MAJ>[nom]</MAJ> adipiscing elit. Curabitur feugiat.'
deviendra
'Lorem nom dolor sit amet, NOM adipiscing elit. Curabitur feugiat.'

Les variables commençant par "&" sont celles définies dans dyn/varpdf.inc
(exemple &aujourdhui) et dans la table om_parametre.



Paramétrer des lettres-type
---------------------------

Il est conseillé d'utiliser l'assistant lettre-type du générateur.

Les paramètres sont les suivants ::

    orientation portrait ou paysage
    format="A4", A3
    position et nom  du logo 
    titre de la lettre
    position et caractéristiques du titre
    corps de la lettre
    position et caractéristiques du corps
    la requête SQL


Pour le corps et le titre, les zones entre crochets  sont les champs de fusion,
sélectionnés par la requête.

Les variables commençant par "&" sont celles définies dans
dyn/varlettretypepdf.inc (exemple &aujourdhui) et dans la table om_parametre.


Les sous-états
--------------

Il est conseillé d'utiliser l'assistant sous-etat du générateur.

Les paramètres  sont les suivants ::

    texte et caractéristique du Titre
    Intervalle avant et après le tableau
    Entête de tableau (nom de colonne)
    caractéristique du tableau
    caractéristique des cellules
    total, moyenne, nombre
    requête SQL


Pour le titre, les zones entre crochets sont les champs de fusion,
sélectionnés par la requête.

Les variables commençant par "&" sont celles définies dans dyn/varpdf.inc
(exemple &aujourdhui) et dans la table om_parametre.


Les champs de fusion
--------------------




Les variables de remplacement
-----------------------------

Lorsque dans les zones de remplacement des éditions, une chaîne de caractère commençant par "&" est identifiée elle essaye d'être remplacée. Ces éléments sont nommées variables de remplacement. Elles peuvent provenir de trois sources différentes : 

- les fichiers de configuration ``../dyn/var*pdf.inc``,
- les méthodes globales de la classe du fichier ``../obj/om_dbform.class.php``,
- la table de paramètres ``om_parametre``.


Les fichiers de configuration ``../dyn/var*pdf.inc``
====================================================


Les méthodes globales de la classe du fichier ``../obj/om_dbform.class.php``
============================================================================


La table de paramètres ``om_parametre``
=======================================




Les requêtes
------------



- ``obj/om_requete.class.php``
- ``core/obj/om_requete.class.php``
- ``gen/obj/om_requete.class.php``
- ``sql/pgsql/om_requete.form.inc.php``
- ``sql/pgsql/om_requete.inc.php``
- ``core/sql/pgsql/om_requete.form.inc.php``
- ``core/sql/pgsql/om_requete.inc.php``
- ``gen/sql/pgsql/om_requete.form.inc.php``
- ``gen/sql/pgsql/om_requete.inc.php``


Les logos
---------

- ``obj/om_logo.class.php``
- ``core/obj/om_logo.class.php``
- ``gen/obj/om_logo.class.php``
- ``sql/pgsql/om_logo.form.inc.php``
- ``sql/pgsql/om_logo.inc.php``
- ``core/sql/pgsql/om_logo.form.inc.php``
- ``core/sql/pgsql/om_logo.inc.php``
- ``gen/sql/pgsql/om_logo.form.inc.php``
- ``gen/sql/pgsql/om_logo.inc.php``




L'éditeur WYSIWYG
-----------------

Description de l'intégration de TinyMCE et des différents configurations.


==========
Composants
==========

Les scripts du framework qui s'occupent de la gestion des éditions sont :

- ``core/fpdf_etat.php``
- ``core/fpdf_etiquette.php``
- ``core/db_fpdf.php``
- ``core/om_edition.class.php``
- ``scr/edition.php``


Les librairies PHP sont :

- ``php/fpdf/``
- ``php/tcpdf/``

