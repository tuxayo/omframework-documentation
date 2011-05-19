.. _index:



openMairie_exemple est le framework de base dans lequel vous pouvez
développer votre propre application.
    

openMairie_exemple est **téléchargeable sur le site de l'adullact**

http://adullact.net/frs/?group_id=329


Il est proposé ici de décrire le fonctionnement du framework.

Dans un environnement LAMP/WAMP, le framework integre des composants au travers
de classes qui permettent de créer des formulaires et des états.
Ces classes sont surchargées par les objets métier à créer.

openMairie integre de nombreux composants : DBPEAR et FPDF dans toutes les applications,
mais aussi ARTISHOW (pour les graphes), NSOAP pour les web services,
JQUERY pour l'ergonomie, OPENLAYERS pour l interface SIG ...

DBPEAR est un abstracteur de base de données qui permet d'utiliser diverses bases de données notament MYSQL ou POSTGRESQL.

FPDF est le composant qui permet de gérer le PDF.


Le développement consiste à créer des objets métier  qui surchargent la classe abstraite
dbformdyn.class.php (composant openMairie). De base, les données de la base de données sont récupérés pour le
formulaire (longueur, max, nom).

- dbformdyn.class.php ; assure la liaison entre le formulaire et la base de données

- formulairedyn.class.php : rassemble toutes les méthodes permettant de construire des formulaires ::




    Ce chapître propose de vous décrire les outils de base du framework de la manière suivante :
    
        - le paramétrage général du framework
        - la gestion des accès du framework et la multi collectivite
        - les méthodes pour construire des formulaires avec le framework
        - les outils d'édition du framework
        - l'outil de requête paramétrable du framework
        - l'ergonomie intégrant jquery
        - la gestion de traitement et la construction de programme spécifiques avec les utilitaires
        - l'import des données CSV du framework
        - l'interface geographique intégrant openlayers avec openCimetiere
        - l'envoi de mail intégrant phpmail avec openPersonnalite (en projet)
        - le fonctionnement d'artichow dans openResultat (en projet)
        - le fonctionnement du nusoap dans openFoncier (enprojet)
        - l'utilisation de fpdf pour écrire dans un pdf dans openCourrier (en projet)
        - la mise en oeuvre d'un éditeur wysiwyg pour l'édition des états (en projet)



.. toctree::

    parametrage.rst
    affichage.rst
    formulaire.rst
    methode.rst
    edition.rst
    reqmo.rst
    acces.rst
    ergonomie.rst
    utilitaire.rst
    import.rst
