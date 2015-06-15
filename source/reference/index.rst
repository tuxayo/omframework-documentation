.. _reference:

###################
Manuel de référence
###################

openMairie_exemple est le framework de base dans lequel vous pouvez développer votre propre application.

openMairie_exemple est `téléchargeable sur le site de l'adullact`_.

.. _téléchargeable sur le site de l'adullact: http://adullact.net/frs/?group_id=329

Il est proposé ici de décrire le fonctionnement du framework.

"En programmation orientée objet un framework est typiquement composé de classes mères qui seront dérivées et étendues par héritage en fonction des besoins spécifiques à
chaque logiciel qui utilise le framework".

http://fr.wikipedia.org/wiki/Framework

Dans un environnement LAMP/WAMP, le framework openMairie intègre des composants au travers de classes qui permettent de créer des formulaires et des états. Ces classes sont surchargées par les objets métier à créer.

openMairie intègre de nombreux composants : DBPEAR et FPDF dans toutes les applications, JQUERY pour l'ergonomie, OPENLAYERS pour l'interface SIG ...

DBPEAR est un abstracteur de base de données qui permet d'utiliser diverses bases de données notamment MYSQL ou POSTGRESQL.

FPDF est le composant qui permet de gérer le PDF.


Le développement consiste à créer des objets métier  qui surchargent la classe abstraite
om_dbformdyn.class.php (composant openMairie). De base, les données de la base de données sont récupérées pour le
formulaire (longueur, max, nom).

- om_dbformdyn.class.php ; assure la liaison entre le formulaire et la base de données
- om_formulaire.class.php : rassemble toutes les méthodes permettant de construire des formulaires


Ce chapitre propose de vous décrire les outils de base du framework de la manière suivante :

    - le paramétrage général du framework en /dyn
    - les méthodes pour construire des formulaires avec le framework
    - les outils d'édition du framework
    - l'outil de requête paramétrable du framework
    - la gestion des accès du framework ainsi que la gestion multi-collectivite
    - l'ergonomie intégrant jquery
    - la gestion de traitement et la construction de programme spécifiques avec les utilitaires
    - l'import des données CSV du framework

.. toctree::
   :numbered:
   
   arborescence.rst
   initialisation_base_de_donnees.rst
   parametrage.rst
   affichage.rst
   advancedsearch.rst
   formulaire.rst
   actions-form.rst
   methode.rst
   acces.rst
   dashboard.rst
   edition.rst
   import.rst
   reqmo.rst
   sig.rst
   layout.rst
   filestorage.rst
   tests_ci/index.rst
   utilitaire.rst
