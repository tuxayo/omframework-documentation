 .. _tests_ci:

#############################
Tests et Intégration Continue
#############################


==========
Pré-requis
==========

PHPUnit
-------

https://phpunit.de


Installation
============

.. code-block:: sh

  wget https://phar.phpunit.de/phpunit.phar
  chmod +x phpunit.phar
  sudo mv phpunit.phar /usr/local/bin/phpunit


Selenium Server
---------------

http://www.seleniumhq.org

Installation
============

.. code-block:: sh

  wget http://selenium-release.storage.googleapis.com/2.45/selenium-server-standalone-2.45.0.jar
  java -jar selenium-server-standalone-2.45.0.jar


Robot Framework
---------------

http://robotframework.org/


Installation
============

.. code-block:: sh

  sudo apt-get install python python-dev python-pip
  sudo pip install robotframework robotframework-selenium2library robotframework-selenium2screenshots requests robotframework-requests Pillow


.. warning::

  La librairie selenium2screenshots doit être modifiée pour fonctionner correctement.
  
  vim /usr/local/lib/python2.7/dist-packages/Selenium2Screenshots/keywords.robot
  
  .. code-block:: sh
  
    :%s/u'  '/u'  '  count=1/g


=============================
Fonctionnement et Utilisation
=============================

Pré-requis
----------

Les tests doivent être joués dans un environnement balisé et reproductible à
l'identique. Pour ce faire il est nécessaire avant chaque lancement de test,
de dérouler une routine qui permet de mettre en place un environnement de tests. 
Un script permet de dérouler cette routine de manière automatisée : 

.. code-block:: sh

  ./om-tests -c initenv


Ce script permet de :

* supprimer la base de données
* créer la base de données
* initialiser la base de données grâce au script data/pgsql/install.sql
* redémarrer apache pour prendre les traductions en compte
* donner les droits à apache pour les dossiers dans lequel il peut écrire
* faire un lien symbolique vers le dossier de l'applicatif pour que les tests
  en question dans le dossier /var/www/


Tous les tests
--------------

Lancer tous les tests avec initialisation de l'environnement de tests

.. code-block:: sh

  ./om-tests -c runall


Un seul TestSuite
-----------------

Lancer un TestSuite avec initialisation de l'environnement de tests

.. code-block:: sh

  ./om-tests -c runone -t 000_testsuite_a_executer.robot

Lancer un TestSuite sans initialisation de l'environnement de tests

.. code-block:: sh

  ./om-tests -c runone -t 000_testsuite_a_executer.robot --noinit


=================================
Développement et bonnes pratiques
=================================

Il est prévu de consigner ici les bonnes pratiques et les consignes pour le développement des tests.

Documentation RobotFramework
----------------------------


Librairie du framework openMairie `Core <https://scm.adullact.net/anonscm/svn/openmairie/openmairie_exemple/trunk/tests/doc/core.html>`_.

.. raw:: html

   <iframe src="https://scm.adullact.net/anonscm/svn/openmairie/openmairie_exemple/trunk/tests/doc/core.html" width="100%" height="500px"></iframe>


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
