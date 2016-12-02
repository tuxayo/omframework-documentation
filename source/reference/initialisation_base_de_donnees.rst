.. _initialisation_base_de_donnees:

####################################
Initialisation de la base de données
####################################

.. contents::

======================================
Description du dossier ``data/pgsql/``
======================================

Il est nécessaire de positionner l'entête de fichier suivant pour chacun des
fichiers sql de ce dossier : ::

    --------------------------------------------------------------------------------
    -- Description succinte de l'utilité du fichier
    --
    -- Informations nécessaires à la génération ou à la composition du fichier
    --
    -- @package <APPLICATIF>
    -- @version SVN : $Id$
    --------------------------------------------------------------------------------


Description de tous les fichiers ``init*.sql``
----------------------------------------------

Il s'avère nécessaire de mettre dans l'entête des fichiers ``init*.sql`` la
commande ou les instructions qui ont permis de générer ou de composer le fichier
en question.


Le fichier ``init.sql``
.......................

Ce fichier contient les instructions de base du framework. Il permet de créer
les tables et les séquences du framework (celles qui commencent par `om_*`). Il
est généré grâce à la commande : ::

    pg_dump -s -O -n <SCHEMA> -T <SCHEMA>.om_* <DATABASE>

Dans les applicatifs, ce fichier est sensé être directement copié depuis le
framework.


Les fichiers ``init_metier*.sql``
.................................

Ces fichiers contiennent les instructions de base de l'applicatif.

* Le fichier ``init_metier.sql`` permet de modifier (si besoin) le modèle de
  données du framework et de créer les tables et les séquences de l'applicatif
  (celles qui ne commencent par `om_*`). Il est généré grâce à la commande : ::

      pg_dump -s -O -n <SCHEMA> -t <SCHEMA>.om_* <DATABASE>

  Dans le framework, ce fichier est vide.

* Le fichier ``init_metier_sig.sql`` permet de modifier le modèle de données
  créé précédemment pour y ajouter des champs de type geom pour la gestion
  du SIG.

* Le fichier ``init_metier_vue.sql`` permet de modifier le modèle de données
  créé précédemment pour y remplacer une table par une vue vers la table d'un
  autre schéma ou d'une autre base de données.


Les fichiers ``init_parametrage*.sql``
......................................

Ces fichiers contiennent l'initialisation du paramétrage c'est-à-dire les
données nécessaires à l'utilisation de l'application. Ils sont générés
généralement grâce à la commande : ::

    pg_dump -a -t <SCHEMA>.<TABLE1> -t <SCHEMA>.<TABLE2> ... <DATABASE>


* Le fichier ``init_parametrage.sql`` permet d'initialiser par exemple
  la ou les collectivités de base ainsi que les profils et l'utilisateur admin.
  Dans certains applicatifs simple, ce fichier peut être unique et tout le
  paramétrage contenu dans ce dernier.

* Le fichier ``init_parametrage_permissions.sql`` permet d'initialiser
  les permissions de l'applicatif. Cette initialisation se trouve dans
  un fichier séparé pour appréhender plus facilement le paramétrage des
  permissions et éventuellement la mise à jour de ce paramétrage.

* Le fichier ``init_parametrage_editions.sql`` permet d'initialiser les
  editions de base générique de l'aplicatif. Cette initialisation se trouve dans
  un fichier séparé pour appréhender plus facilement le paramétrage des
  éditions et éventuellement la mise à jour de ce paramétrage.
  
* Le fichier ``init_parametrage_*.sql`` peut permettre de découper
  encore l'initialisation pour appréhender plus facilement le paramétrage et
  éventuellement la mise à jour de ce paramétrage.


Le fichier ``init_data.sql``
............................

Ce fichier contient l'initialisation d'un jeu de données à destination de trois
environnements distincts :

* l'environnement de développement,
* l'environnement de démonstration,
* l'environnement de tests.


Description des fichiers ``vX.X.X.sql`` ou ``ver_X.X.X.sql``
------------------------------------------------------------

Ces fichiers permettent de mettre à jour les applicatifs d'une version vers
la version supérieure. Le X.X.X correspond au numéro de version vers lequel
la mise à jour se fait et depuis la version juste précédente.

Lorsque le framework ou l'applicatif est en développement, ce fichier peut être
suffixé par `-dev` et indique qu'il n'a pas encore été intégré aux différents
fichiers ``init*.sql``. Juste avant une nouvelle version du framework, les
fichiers ``init*.sql`` doivent être regénérés pour intégrés les dernières
modifications et ce fichier renommé avec son nom ``vX.X.X.sql`` ou
``ver_X.X.X.sql`` standard.


Description du fichier ``update_sequences.sql``
-----------------------------------------------

Ce fichier permet de créer une fonction capable de mettre à jour toutes les
séquences correctement liées aux champs auxquels elles se rattachent en
fonction de la dernière valeur du champ dans la table. En plus de la création
de la fonction ce script exécute la fonction.


Description du fichier ``install.sql``
--------------------------------------

Ce fichier permet d'initialiser tous les fichiers qui sont décrits ci-dessus
dans le bon ordre. Par défaut ce fichier installe la base de données et les
données nécessaires aux trois environnements suivants :

* l'environnement de développement,
* l'environnement de démonstration,
* l'environnement de tests.


.. note::

   Ce fichier comporte l'initialisation des commandes postgis par défaut pour
   la dernière version de postgis. Les commandes pour l'ancienne version sont
   présentes et commentées dans ce même fichier.


.. _parametrage_connexion_base_de_donnees:

================================================
Paramétrage de la connexion à la base de données
================================================

Le paramétrage de la connexion à la base de données se fait dans le fichier
``dyn/database.inc.php``.

.. note::

   Dans le framework le schéma utilisé par défaut est `openexemple`, dans les
   applicatifs c'est normalement le nom de l'applicatif `<APPLICATIF>`
   (par exemple : `openelec`, `opencimetiere`, ...).

.. code-block:: php

    <?php
    /**
     * Ce fichier permet le paramétrage de la connexion à la base de données,
     * chaque entrée du tableau correspond à une base différente. Attention
     * l'index du tableau conn représente l'identifiant du dossier dans lequel
     * seront stockés les fichiers propres a cette base dans l'application.
     * 
     * @package openmairie_exemple
     * @version SVN : $Id: database.inc.php 2302 2013-05-23 18:04:22Z fmichon $
     */
    
    // PostGreSQL
    $conn[1] = array(
        "openExemple", // Titre 
        "pgsql", // Type de base
        "pgsql", // Type de base
        "postgres", // Login
        "postgres", // Mot de passe
        "tcp", // Protocole de connexion 
        "localhost", // Nom d'hote
        "5432", // Port du serveur
        "", // Socket
        "openexemple", // Nom de la base
        "AAAA-MM-JJ", // Format de la date
        "openexemple", // Nom du schéma
        "", // Préfixe
        NULL, // Paramétrage pour l'annuaire LDAP
        "mail-default", // Paramétarge pour le serveur de mail
        "filestorage-default", // Paramétrage pour le stockage des fichiers
        "extras" => array( // Paramétrage optionnel, utilisé pour les plugins.
            "sig" => "",
        ),
    );
    
    ?>


La documentation de DB PEAR qui est le module d'abstraction utilisé par le
framework donne plus d'informations sur les paramètres.

