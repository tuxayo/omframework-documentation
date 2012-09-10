.. _framework:

############
Le framework
############

openMairie_exemple est le framework de base dans lequel vous pouvez
développer votre propre application.

openMairie_exemple est `téléchargeable sur le site de l'adullact`_.

.. _téléchargeable sur le site de l'adullact: http://adullact.net/frs/?group_id=329

Il est proposé ici de décrire le fonctionnement du framework.

"En programmation orientée objet un framework est typiquement composé de classes mères qui seront dérivées et étendues par héritage en fonction des besoins spécifiques à
chaque logiciel qui utilise le framework".

http://fr.wikipedia.org/wiki/Framework

Dans un environnement LAMP/WAMP, le framework openMairie integre des composants au travers
de classes qui permettent de créer des formulaires et des états.
Ces classes sont surchargées par les objets métier à créer.

openMairie integre de nombreux composants : DBPEAR et FPDF dans toutes les applications,
mais aussi ARTISHOW (pour les graphes), JQUERY pour l'ergonomie, OPENLAYERS pour l interface SIG ...

DBPEAR est un abstracteur de base de données qui permet d'utiliser diverses bases de données notament MYSQL ou POSTGRESQL.

FPDF est le composant qui permet de gérer le PDF.


Le développement consiste à créer des objets métier  qui surchargent la classe abstraite
om_dbformdyn.class.php (composant openMairie). De base, les données de la base de données sont récupérées pour le
formulaire (longueur, max, nom).

- om_dbformdyn.class.php ; assure la liaison entre le formulaire et la base de données
- om_formulaire.class.php : rassemble toutes les méthodes permettant de construire des formulaires

En introduction, il est proposé une explication de la hiérarchie des répertoires ::
    
    - app : contient tout ce qui est spécifique de votre application avec les
    javacripts (app/js/script.js) et les images (app/img)
    
    - css : contient les feuilles de style de base du framework

    - core : classe openMairie (version 4.2.0)
    
    - data : contient les requêtes sql permettant de créer votre application
            pgsql : pour postgresql
            mysql pour mysql
        dans les 2 cas, init.sql permet de créer les tables om_xx du framework
                        init_metier.sql sont les tables spécifiques de votre application
        d'autres scripts complètent la mise en oeuvre des données notament les
        modifications de base de données.
        
    - dyn : contient les fichiers de paramètrage du framework décrit ci dessous
    
    - gen : contient les scripts (obj) et requetes (sql) générées par le générateur
    
    - img : contient les images du framework 
    
    - js : contient les javascripts de base du framework
    
    - lib : contient les librairies javascripts : openLayers et jquery
    
    - locales : contient les traductions de l'application
    
    - obj : contient les objets métiers surchargeant les objets générés (gen/obj)
    
    - pdf : contient les scripts d'édition du framework
    
    - php : contient les librairies php utilisées par le framework notament : dbpear, phpmailer et fpdf
    
    - scr : contient les scripts d'affichage du framework
    
    - spg : contient les sous programmes génériques du framework
    
    - sql : contient les scripts sql surchargeant les scripts générés (gen/sql ...)
    
    - tmp : contient les fichiers temporaires créés par l'application
    
    - trs : contient les fichiers uploadés de l'application (par base /1 /2 ...)
    
    Les repertoires à modifier pour une application sont les suivants :
    
    - app : vos scripts spécifiques
    - dyn : le paramètrage du framework et de l application
    - gen : les scripts php et sql générés
    - obj : vos objets métiers en surcharge
    - sql : les requetes sql surchargées
    - tmp : les  fichiers temporaires
    - trs : les fichiers transférés en upload.

Ce chapître propose de vous décrire les outils de base du framework de la manière suivante :

    - le paramétrage général du framework en /dyn
    - les méthodes pour construire des formulaires avec le framework
    - les outils d'édition du framework
    - l'outil de requête paramétrable du framework
    - la gestion des accès du framework ainsi que la gestion multi collectivite
    - l'ergonomie intégrant jquery
    - la gestion de traitement et la construction de programme spécifiques avec les utilitaires
    - l'import des données CSV du framework

.. toctree::
   :numbered:
   
   parametrage.rst
   affichage.rst
   advancedsearch.rst
   formulaire.rst
   actions-form.rst
   methode.rst
   edition.rst
   reqmo.rst
   acces.rst
   tdb.rst
   ergonomie.rst
   utilitaire.rst
   import.rst
