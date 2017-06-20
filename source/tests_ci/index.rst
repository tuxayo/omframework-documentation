 .. _tests_ci:

#############################
Tests et Intégration Continue
#############################


=====
Tests
=====

Pré-requis
==========

Pour les tests, deux librairies sont utilisées :

* PHPUnit : https://phpunit.de
* Robot Framework : http://robotframework.org

Pour le tests d'envoi de courriel, un server STMP local est intégré à om-tests intégré : 

* maildump : https://pypi.python.org/pypi/maildump

.. warning::

  Il faut utiliser la version 2.7 de python.

Installation
============


Principe
--------

Afin de ne pas pertuber l'environnement python système, et conserver la cohérence des dépendances, 
il est conseillé d'installer les librairies nécessaires et leurles dépendances dans un environment cloisonné.
Pour cela, on utilise *Virtualenv*. Les libraires et les binires seront déployés en espace utilisateur, il faudra activer le *Virtualenv* pour pouvoir lancer les tests.

Pour la suite, les exemples de codes sont basé sur un environnement Ubuntu 16.04.
On suppose également qu'il existe une copie locale de *openmairie_exemple* disponible dans `~/openmairie`.

Installation des paquets de base
--------------------------------

.. code-block:: sh

  sudo apt install python python-dev python-pip libjpeg-dev
  sudo -H pip install virtualenv


Création de l'environnement
---------------------------

.. code-block:: sh

  virtualenv om-tests
  cd om-tests
  . bin/activate


Installation des librairies python
----------------------------------

.. code-block:: sh

  (om-tests) pip install -r ~/openmairie/tests/pip-requirements.txt

.. warning::

  La librairie selenium2screenshots est bugguée et doit être modifiée pour fonctionner correctement :
  
  .. code-block:: sh

    (om-tests) vim lib/python2.7/site-packages//Selenium2Screenshots/keywords.robot
    (om-tests) :%s/u'  '/u'  '  count=1/g


Installation de PHPUnit
-----------------------

Bien que ce ne soit pas une librairie python, on la déploie dans le *virtualenv*. L'activation du *vitualenv* va rajouter le chemin vers `om-tests/bin` dans la variable `PATH` de l'utilisateur.

.. code-block:: sh

  (om-tests) cd bin
  (om-tests) wget -O phpunit https://phar.phpunit.de/phpunit.phar
  (om-tests) chmod +x phpunit




Arborescence du répertoire `tests`
==================================

::

  .
  ├── [drwxrwxr-x]  binary_files
  ├── [drwxrwxr-x]  doc
  ├── [drwxrwxr-x]  results
  ├── [drwxrwxr-x]  resources
  │   ├── [drwxrwxr-x]  app
  │   │   ├── [drwxrwxr-x]  gen
  │   │   ├── [-rw-rw-r--]  __init__.py
  │   │   ├── [-rw-rw-r--]  om_tests.py
  │   │   └── [-rw-rw-r--]  keywords.robot
  │   ├── [drwxrwxr-x]  core
  │   ├── [-rw-rw-r--]  __init__.py
  │   └── [-rw-rw-r--]  resources.robot
  ├── [-rwxrwxr-x]  om-tests
  ├── [-rw-rw-r--]  config.xml
  ├── [-rw-rw-r--]  000_generation.robot
  └── [-rw-rw-r--]  000_test_unitaire.php


`tests/om-tests`
----------------

Ce fichier doit être exécutable.

.. code-block:: python

  #!/usr/bin/python
  from resources.app.om_tests import om_tests
  tests = om_tests()
  tests.main()


`tests/config.xml`
------------------

.. code-block:: xml

  <phpunit>
    <testsuites>
      <testsuite name="openmairie">
          <file>000_test_unitaire.php</file>
      </testsuite>
    </testsuites>
  </phpunit>


`tests/000_generation.robot`
----------------------------

.. code-block:: xml

  *** Settings ***
  Resource  resources/resources.robot
  Suite Setup  For Suite Setup
  Suite Teardown  For Suite Teardown
  Documentation  Le 'Framework' de l'application permet de générer
  ...  automatiquement certains scripts en fonction du modèle de données. Lors
  ...  du développement la règle est la suivante : toute modification du
  ...  modèle de données doit entrainer une regénération complète de tous les
  ...  scripts. Pour vérifier à chaque modification du code que la règle a bien
  ...  été respectée, ce 'Test Suite' permet de lancer une génération complète.
  ...  Si un fichier est généré alors le test doit échoué.


  *** Test Cases ***
  Génération complète

      Depuis la page d'accueil    admin    admin
      Générer tout


`tests/000_test_unitaire.php`
-----------------------------

.. code-block:: php

  <?php
  class General extends PHPUnit_Framework_TestCase {

      /**
       * Méthode lancée en début de traitement
       */
      public function setUp() {
      }

      /**
       * Méthode lancée en fin de traitement
       */
      public function tearDown() {
      }

      /**
       * Test Case n°01
       */
      public function test_case_01() {
          require_once "../obj/utils.class.php";
          @session_start();
          $_SESSION['collectivite'] = 1;
          $_SESSION['login'] = "admin";
          $_SERVER['REQUEST_URI'] = "";
          $f = new utils("nohtml");
          $f->disableLog();

          $this->assertEquals($year, 2015);

          $f->__destruct();
      }
  }
  ?>


`tests/doc/`
------------

Répertoire destiné à recevoir la génération de la documentation des mots clés Robot Framework. 


`tests/results/`
----------------

Répertoire destiné à recevoir la génération des rapports et des captures d'écran produits pendant l'exécution des tests. Afin que ces nouveaux fichiers ne gênent pas l'utilisation des commandes Subversion, tous les fichiers à l'intérieur de ce dossier sont ignorés grâce à la propriété svn:ignore.


`tests/binary_files/`
---------------------

Répertoire destiné à recevoir les fichiers de configuration ou d'initialisation de l'environnement de tests.


`tests/resources/`
------------------

Répertoire contenant les ressources utilisées par les tests suite.


`tests/resources/__init__.py`
-----------------------------

Fichier vide pour définir le répertoire `resources` comme un module python.


`tests/resources/resources.robot`
---------------------------------

.. code-block:: xml

  *** Settings ***
  #
  Resource          core${/}om_resources.robot
  #
  Resource          app${/}keywords.robot

  *** Variables ***
  ${SERVER}           localhost
  ${PROJECT_NAME}     openexemple
  ${BROWSER}          firefox
  ${DELAY}            0
  ${ADMIN_USER}       admin
  ${ADMIN_PASSWORD}   admin
  ${PROJECT_URL}      http://${SERVER}/${PROJECT_NAME}/
  ${PATH_BIN_FILES}   ${EXECDIR}${/}binary_files${/}
  ${TITLE}            :: openMairie :: openexemple

  *** Keywords ***
  For Suite Setup
      # Les keywords définit dans le resources.robot sont prioritaires
      Set Library Search Order    resources
      Ouvrir le navigateur
      Tests Setup


  For Suite Teardown
      Fermer le navigateur


`tests/resources/app/`
----------------------

Répertoire contenant les fichiers de déclaration de mots clé dédiés à l'application.


`tests/resources/app/gen/`
--------------------------

Répertoire destiné à recevoir des fichiers de mots clé générés à partir du modèle de données.


`tests/resources/app/__init__.py`
---------------------------------

Fichier vide pour définir le répertoire `app` comme un module python.


`tests/resources/app/om_tests.py`
---------------------------------

.. code-block:: python

  #!/usr/bin/python
  # -*- coding: utf-8 -*-
  from resources.core.om_tests import om_tests_core


  class om_tests(om_tests_core):
      """
      """

      _database_name_default = "openexemple"
      _instance_name_default = "openexemple"


`tests/resources/app/keywords.robot`
------------------------------------

.. code-block:: xml

  *** Settings ***
  Documentation   Keywords openexemple.

  *** Keywords ***
  Depuis le listing
      [Documentation]
      [Arguments]  ${listing_obj}
      Go To  ${PROJECT_URL}scr/tab.php?obj=${listing_obj}


`tests/resources/core/`
-----------------------

Répertoire récupéré depuis le core du framework via un EXTERNALS.

.. code-block:: xml

  tests/resources/core/  svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/trunk/tests/resources/core/


Fonctionnement et Utilisation
=============================

Pré-requis
----------

Toute les opérations suivantes vont faire appel aux binaires et libraires déployés dans l'environnement de test. Il faut donc qu'il soit activé :

.. code-block:: sh

  cd om-tests
  . bin/activate


Les tests doivent être joués dans un environnement balisé et reproductible à
l'identique. Pour ce faire il est nécessaire avant chaque lancement de test,
de dérouler une routine qui permet de mettre en place un environnement de tests. 
Un script permet de dérouler cette routine de manière automatisée : 


.. code-block:: sh

  (om-tests) ./om-tests -c initenv


Ce script permet de :

* supprimer la base de données
* créer la base de données
* initialiser la base de données grâce au script data/pgsql/install.sql
* redémarrer apache pour prendre les traductions en compte
* donner les droits à apache pour les dossiers dans lequel il peut écrire
* faire un lien symbolique vers le dossier de l'applicatif pour que les tests puisse y accéder depuis le dossier /var/www/
* appliquer les opération d 'initialisation précisées dans *resources/app/om_tests.py*

Les tests sont prévus pour être exécutés sur le navigateur Firefox. Il est possible d'utiliser une version spécifique automatiquement lors de l'execution des tests.
Pour définir une version de navigateur spécifique il faut :

* télécharger le navigateur Firefox conseillé :

    * `64 bits <https://ftp.mozilla.org/pub/firefox/releases/31.2.0esr/linux-x86_64/fr/firefox-31.2.0esr.tar.bz2>`_
    * `32 bits <https://ftp.mozilla.org/pub/firefox/releases/31.2.0esr/linux-i686/fr/firefox-31.2.0esr.tar.bz2>`_ 

* extraire l'application dans le dossier souhaité
* créer un fichier de configuration dans votre dossier utilisateur :

.. code-block:: sh

  vim ~/.om-tests/config.cfg
  [browser]
  src_path=[chemin du navigateur spécifique]
  dest_path=/usr/local/bin/firefox
  

Tous les tests
--------------

Lancer tous les tests avec initialisation de l'environnement de tests

.. code-block:: sh

  (om-tests) ./om-tests -c runall


Un seul TestSuite
-----------------

Lancer un TestSuite avec initialisation de l'environnement de tests

.. code-block:: sh

  (om-tests) ./om-tests -c runone -t 000_testsuite_a_executer.robot

Lancer un TestSuite sans initialisation de l'environnement de tests

.. code-block:: sh

  (om-tests) ./om-tests -c runone -t 000_testsuite_a_executer.robot --noinit

.. todo::
  usage de maildump

Serveur SMTP local
------------------

Le server STMP local (*maildump*) est intégré à *om-tests*. A chaque lancement de tests, il est démarré, puis arrêté à la fin de l'exécution de ceux-ci.
La configuration mail adéquate est gérée par le *initenv*.

Il peut également être lancé à la demande.

.. warning::

  Le serveur SMTP tourne sur le port 1025, il doit donc être disponible sur la machine.

Démarrage
+++++++++

.. code-block:: sh

  (om-tests) ./om-tests -c startsmtp

Arrêt
+++++

.. code-block:: sh

  (om-tests) ./om-tests -c stoptsmtp

Interface web
+++++++++++++

*maildump* fourni également un interface web, dans laquelle les courriels envoyés peuvent être consultés.
Cette interface est accessible dans un navigateur à l'URL suivante ::

  http://localhost:1080


Développement et bonnes pratiques
=================================

Il est prévu de consigner ici les bonnes pratiques et les consignes pour le développement des tests.

Documentation RobotFramework
----------------------------


Librairie du framework openMairie `Core <https://scm.adullact.net/anonscm/svn/openmairie/openmairie_exemple/trunk/tests/doc/core.html>`_.

.. raw:: html

   <iframe src="https://scm.adullact.net/anonscm/svn/openmairie/openmairie_exemple/trunk/tests/doc/core.html" width="100%" height="500px"></iframe>

Cette documentation de la librairie du framework openMairie a été générée avec la commande suivante :

.. code-block:: sh

  (om-tests) ./om-tests -c gendoc

La commande est automatiquement exécutée lorsque l'on lance un ou tous les TestSuite.
La documentation est générée au format HTML dans le répertoire *tests/doc*.
Il y a une documentation par dossier de ressources :

  - *tests/resources/app* → *tests/doc/app.html*
  - *tests/resources/core* → *tests/doc/core.html*


RobotFramework :

- http://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html


Librairies :

- Base - BuiltIn : http://robotframework.org/robotframework/latest/libraries/BuiltIn.html
- Base - String : http://robotframework.org/robotframework/latest/libraries/String.html
- Base - Collections : http://robotframework.org/robotframework/latest/libraries/Collections.html
- Base - OperatingSystem : http://robotframework.org/robotframework/latest/libraries/OperatingSystem.html
- Selenium2 : http://rtomac.github.io/robotframework-selenium2library/doc/Selenium2Library.html
- Requests : http://bulkan.github.io/robotframework-requests/
- Selenium2Screenshots : https://robotframework-selenium2screenshots.readthedocs.org/en/latest/_downloads/keywords.html


Convention de nommage
---------------------

* Un fichier de test par thème fonctionnel, une TestCase par fonctionnalité.
* Convention de nommage :
    * des fichiers : mon_theme_fonctionnel.robot
    * des testcase : Saisir un nouvel élément

.. _generation_robot_framework:

Génération
----------

Pré-requis : créer le dossier 'gen' dans '../tests/resources/core/gen/'.

Lancer une génération complète à chaque modification de la structure de la base
de données permet de créer les mots-clefs basiques de chaque table : "depuis le
contexte", "ajouter", "modifier", "supprimer" et "saisir".

Bonnes pratiques
----------------

* Éviter d'utiliser les sélecteurs XPATH, les sélecteurs CSS ou par ID sont largement préférables.
* Isolation des tests : chacun des tests ajouté doit être indépendant de ceux existants (consitution de son propre jeu de données, accès aux éléments par recherche, éventuellement nettoyage des données crées, etc).

====================
Intégration continue
====================

Jenkins
=======

http://jenkins.openmairie.org