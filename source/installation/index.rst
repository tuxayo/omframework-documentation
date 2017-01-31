.. _installation:

#########################
Prérequis et installation
#########################


.. contents::


**********
Pré-requis
**********

Vous devez avoir installé :

- un serveur web (apache, ...)
- PHP (Versions testées : 5.5 > 5.6)
- le moteur de base de donnees PostGreSQL (Versions testées : 9.1 > 9.4) avec l'extension PostGIS (Versions testées : 2.0 > 2.1)
- la librairie XML : libxml > 2.9.0


Sous windows, il est facile de trouver de la documentation pour l'installation
de ces éléments en utilisant wamp (http://www.wampserver.com/) par exemple.


Sous Linux, il est facile de trouver de la documentation pour l'installation de
ces éléments sur votre distribution.

Il est nécessaire de configurer l'installation PHP via le script php.ini comme suit pour cacher les erreurs "deprecated" liées à la librairie DBPEAR ::


  error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT
  display_errors = On


************
Installation
************

Installation des fichiers du framework
======================================

Télécharger l'archive zip
-------------------------

http://adullact.net/frs/?group_id=265


Décompresser l'archive zip dans le répertoire de votre serveur web
------------------------------------------------------------------

- Exemple sous windows dans wamp : wamp/www/framework-openmairie
- Exemple sous linux avec debian : /var/www/framework-openmairie


Création et initialisation de la base de données
================================================

Créer la base de données
------------------------

Il faut créer la base de données dans l'encodage UTF8. Par défaut la base de données s'appelle openexemple.


Dans un environnement debian :

.. code-block:: bash

  createdb openexemple


Initialiser la base de données
------------------------------

Il faut initialiser les tables, les séquences et données de paramétrage grâce au script data/pgsql/install.sql


Dans un environnement debian depuis le répertoire data/pgsql/ :

.. code-block:: bash

  psql openexemple -f install.sql


Configuration de l'applicatif
=============================

Création des répertoires dédiés au stockage des données de l'applicatif
-----------------------------------------------------------------------

Il est nécessaire de créer les répertoires suivants pour gérer le stockage des fichiers de logs, des fichiers temporaires et des fichiers stockés. L'arborescence à créer est décrite ci-après : :ref:`arborescence_repertoire_var`.


Positionner les permissions nécessaires au serveur web
------------------------------------------------------

Dans un environnement debian : 

.. code-block:: bash

  chown -R www-data:www-data /var/www/framework-openmairie


Configuration de la connexion à la base de données
--------------------------------------------------

La configuration se fait dans le fichier `dyn/database.inc.php` :

.. code-block:: php

    <?php
    ...
    // PostGreSQL
    $conn[1] = array(
        "Framework openMairie", // Titre 
        "pgsql", // Type de base
        "pgsql", // Type de base
        "postgres", // Login
        "postgres", // Mot de passe
        "tcp", // Protocole de connexion 
        "localhost", // Nom d'hote
        "5432", // Port du serveur
        "", // Socket
        "openexemple", // nom de la base
        "AAAA-MM-JJ", // Format de la date
        "openexemple", // Nom du schéma
        "", // Préfixe
        null, // Paramétrage pour l'annuaire LDAP
        null, // Paramétrage pour le serveur de mail
        null, // Paramétrage pour le stockage des fichiers
    );
    ...
    ?>

**********************
Connexion au framework
**********************

Ouverture dans le navigateur
============================

http://localhost/framework-openmairie/

'localhost' peut être remplacé par l'ip ou le nom de domaine du serveur.


Login
=====

* Utilisateur "administrateur" : 
   - identifiant : admin
   - mot de passe : admin

Le message de bienvenue doit être affiché "Votre session est maintenant ouverte."


***************
En cas d'erreur
***************

Activer le mode debug
=====================

Il est possible d'activer le mode debug pour visualiser les messages d'erreur
détaillés. Dans le fichier `dyn/debug.inc.php`, il faut commenter le mode
production et décommenter le mode debug.

Mode production :

.. code-block:: php

   //define('DEBUG', VERBOSE_MODE);
   //define('DEBUG', DEBUG_MODE);
   define('DEBUG', PRODUCTION_MODE); 

Mode debug :

.. code-block:: php

   //define('DEBUG', VERBOSE_MODE);
   define('DEBUG', DEBUG_MODE);
   //define('DEBUG', PRODUCTION_MODE); 

