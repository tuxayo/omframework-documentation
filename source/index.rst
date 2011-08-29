.. openMairie - Guide du développeur documentation master file, created by
   sphinx-quickstart on Fri Oct  1 11:15:18 2010.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

=================================
openMairie - Guide du développeur
=================================


Ce document a pour but de guider les développeurs dans la mise en oeuvre
d'un projet openMairie.

Avec plus de 30 applications développées pour les collectivités locales accessibles
sur le site http://openmairie.org, nous souhaitons au travers de ce guide , diffuser notre
expérience auprès  des collectivités et des acteurs économiques du libre qui les accompagnent.

C'est donc une méthode conçue au fur à mesure de nos développements que nous vous
proposons de partager et toutes remarques sont les bienvenues, alors n'hésitez pas à nous
en faire part à l'adresse mail suivante : contact@openmairie.org

Nous avons conçu openMairie pour fabriquer une maquette le plus en amont possible
en s'appuyant sur le modèle de données créé dans la base de données et en
intégrant les composants standards du "monde libre".

Cette maquette permet très rapidement d'engager un dialogue participatif avec les
utilisateurs, de  concentrer le développeur uniquement sur le "métier" et de faire valider
par l'utilisateur les évolutions successives.

Si vous débutez, il est préférable de commencer par le chapître
"créer une application" qui permet de prendre en main facilement le générateur et le
framework openMairie en vous guidant pas à pas dans le mise en place d'une
gestion de courrier.

Le chapître sur le "framework" complète l'exemple ci dessus en vous décrivant
le paramètrage, les classes formulaires et éditions du framework. Il a pour but
de vous informer de manière complète sur le fonctionnement du framework

Le chapître consacré au "générateur" décrit dans le détail le fonctionnement
de cet outil et de ses assistants. Cet outil permet de fabriquer la maquette.

La version 4.1.0 permet de construire des applications composites (ou mash up)
en intégrant des contenus venant d'application externes. Cela permet de construire
rapidemment une application à faible grâce à la fusion de multiple service internet

Le chapître consacré a l' "information geographique" décrit dans le détail le
fonctionnement SIG interne d'openMairie combinant les API d'internet avec le framework.

Le chapître sur les widgets décrit le tableau de bord paramétrable et individualisé
par l'utilisateur  permettant l'accès à tout type de fonctions internes ou externes.

Enfin ce document rassemble toutes les règles de codage du projet
openMairie, ainsi que des outils pour aider et guider les développeurs de la
communauté.

Les règles indiquées doivent être appliquées pour qu'un projet puisse
intégrer la distribution openMairie car l'objectif est de faciliter la
lisibilité et la maintenance du code ainsi que la prise en main par les
collectivités.

Bonne lecture !

Cette création est mise à disposition selon le Contrat Paternité-Partage des
Conditions Initiales à l'Identique 2.0 France disponible en ligne
http://creativecommons.org/licenses/by-sa/2.0/fr/ ou par courrier postal à
Creative Commons, 171 Second Street, Suite 300, San Francisco,
California 94105, USA.


   
Créer une application
=====================

.. toctree::
   :maxdepth: 3

   utilisation/index.rst


Le framework
============

.. toctree::

    framework/index.rst



Le générateur
=============

.. toctree::

    generateur/index.rst


L'information geographique
==========================

.. toctree::

    sig/index.rst
    

Le tableau de bord et les widgets
=================================

.. toctree::

    tdb/index.rst



Règles et outils
================

.. toctree::

   regles/index.rst
   versions/index.rst
   outils/index.rst




Contributeurs
=============

* `Florent Michon [ATREAL] <fmichon@atreal.net>`_
* `Francois Raynaud <contact@openmairie.org>`_
