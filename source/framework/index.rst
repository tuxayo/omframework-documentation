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
        - les méthodes pour construire des formulaires avec le framework
        - les outils d'édition du framework
        - l'outil de requête paramétrable du framework
        - la gestion des accès du framework et la multi collectivite)
        - l'ergonomie intégrant jquery
        - la gestion de traitement et la construction de programme spécifiques avec les utilitaires
        - l'import des données CSV du framework
    
    
    L'utilisation de fpdf pour écrire dans un pdf dans openCourrier est
    décrit dans ce chapître bien que non intégré dans le framework
       
    Deux modules du framework intégré dans la version 4.01 sont traités dans un
    chapître à part
    
        - l'information géographique
        - les widgets et le tableau de bord paraùètrable

    Les modules suivants utilisés dans des applications openMairie ne sont pas décrits
    dans ce chapître car ils n'ont pas encore intégrés dans le framework openExemple
    
        - l'envoi de mail intégrant phpmail (openPersonnalite)
        - le fonctionnement des graphiques avec artichow (openResultat)
        - le fonctionnement de webservice avec nusoap (openFoncier 2.xx)
        

        
    Il est fait mention des projets en cours  :
    
        - la mise en oeuvre d'un éditeur wysiwyg pour l'édition des états
        - un installateur paramétrable
        - un bus de données (dans le cadre de la plateforme http://algebrics.fr)
        pour un échange inter application
            
    




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
