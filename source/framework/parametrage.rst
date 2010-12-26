.. _parametrage:

###########
Parametrage
###########

Le paramétrage de l application est dans le répertoire /dyn.

Il est proposé dans ce chapitre de décrire les différents fichiers de paramétrage.

Les fichiers de paramétrage sont les suivants ::

    dyn/database.inc.php           connexion a la base de données
    dyn/menu.inc.php               menu principal à gauche
    dyn/action.inc                 menu haut
    dyn/shortslink.inc             lien sous menu haut
    dyn/tbd.inc                    tableau de bord
    dyn/locales.inc                application
    dyn/config.inc.php             application
    dyn/include.inc.php            chemin d'accès aux librairies
    dyn/debug.inc                  mode debug
    dyn/version.inc                paramétrage de la version
    
    README.txt                     fichiers textes
    HISTORY.txt
    SPECIFIC.txt
    LICENCE.txt
    TODO.txt
    INSTALL.txt
    



===============================
connexion de la base de donnees
===============================

le paramétrage de la connexion se fait dans : *dyn/database.inc.php*

Le parametrage par defaut est dans le tableau $conn[1] pour la base 1 : 

Il peut être paramétrée plusieurs bases : conn[1] , conn[2] ...

conn[1] est un tableau php qui contient les parametres de connexion suivants ::

    'titre          => 'openxxx',       [parametrage openmairie]
    'phptype'       => 'mysql',         mysql ou 'pgsql' [parametrage dbpear]
    'dbsyntax'      => '',              [ne pas changer parametrage dbpear]
    'username'      => 'root',          [par defaut sur wamp easyphp ou lamp /
                                        a voir avec le fournisseur d acces le cas echeant]
    'password'      => ''               [par defaut sur wamp easyphp ou lamp /
                                        a voir avec le fournisseur d acces le cas echeant]                     
    'protocol'      => '',
    'hostspec'      => 'localhost',     [nom de serveur par defaut wamp ou easyphp]
    'port'          => '',              [ne pas changer parametrage dbpear]
    'socket'        => '',              [ne pas changer parametrage dbpear]
    'nom de la base'=> 'openxxx',       [parametrage openmairie]
    'format date'   =>'AAAA-MM-JJ'      [parametrage openmairie ne pas changer]
    'shema'         => ''               ou 'public' pour postgre
    'prefixe'       => '' 

Il est possible de définir tout phptype : mysql, pgsql (postgresql), oci8 pour oracle.

Il faut voir la documentation de DB PEAR qui est le module d'abstraction utilisé
dans openMairie (version 4.0.0)


==============
menu principal
==============

Le paramétrage du menu se fait dans le fichier *dyn/menu.inc.php*.

De base, les rubriques suivantes sont paramétrées ::

    application             vide par défaut, contient l'accès à votre application
    export                  contient le script "edition" qui reprend
                                les éditions pdf des tables
                            contient le menu "reqmo" qui reprend les requêtes
                                mémorisées
    traitement              vide par défaut, cet option contient les scripts de
                                traitement
    parametrage             Cet option contient vos tables de paramétrage
    administration          Les scripts de cet option contiennent tout les scripts
                                du framework pour le paramètrage de la collectivité,
                                des états / sous états  et la gestion des accès                                

Le paramètrage du menu se fait dans $menu.

$menu est le tableau associatif qui contient tout le menu de l'application,
il contient lui meme un tableau par rubrique, puis chaque
rubrique contient un tableau par lien :

Les caracteristiques de ce tableau sont les suivantes :

- tableau rubrik ::

     title (obligatoire)
     description (texte qui s'affiche au survol de la rubrique)
     href (contenu du lien href)
     class (classe css qui s'affiche sur la rubrique)
     right (droit que l'utilisateur doit avoir pour visionner cette rubrique)
     links (obligatoire)

- tableau links ::

     title (obligatoire) 
     href (obligatoire) (contenu du lien href)
     class (classe css qui s'affiche sur l'element)
     right (droit que l'utilisateur doit avoir pour visionner cet element)
     target (pour ouvrir le lien dans une nouvelle fenetre)


=========
menu haut
=========

Le paramétrage du menu haut se fait dans le fichier *dyn/action.inc.php*

Par défaut, il est paramètré :

- le changement de mot de poste

- la deconnexion



$actions est le tableau associatif qui contient tous les liens presents dans
les actions a cote du login et du nom de la collectivite

les caracteristiques du tableau link sont les suivantes :


- tableau link ::

    title (obligatoire)
    description (texte qui s'affiche au survol de l'element)
    href (obligatoire) (contenu du lien href)
    class (classe css qui s'affiche sur l'element)
    right (droit que l'utilisateur doit avoir pour visionner cet element)
    target (pour ouvrir le lien dans une nouvelle fenetre)

Les liens sous le menu des actions se paramètrent dans le fichier : *dyn/shortlinks.inc.php*

$shortlinks est le tableau associatif qui contient tous les liens presents
dans les raccourcis en dessous des actions
 
Par défaut, il est paramètré l'accès au tableau de bord.

Les caracteristiques du tableau $link sont les suivantes :

- tableau link ::

    title [obligatoire]
    description (texte qui s'affiche au survol de l'element)
    href [obligatoire] (contenu du lien href)
    class (classe css qui s'affiche sur l'element)
    right (droit que l'utilisateur doit avoir pour visionner cet element)
    target (pour ouvrir le lien dans une nouvelle fenetre)


===============
tableau de bord
===============

Le tableau de bord se paramètre dans le fichier *dyn/tdb.inc*. 

Le parametrage est libre et depend de l application.

Ce fichier est appellé par le script scr/dashboard.php.

**Exemple de code** ::

    $description = _("Bienvenue ").$_SESSION["login"]."<br>";    
    $f->displayDescription($description);

Ce paramétrage va afficher "bienvenue demo"


================
locales : langue
================

Les variables locales sont paramétrées dans le fichier *dyn/locales.inc.php*

Ce fichier contient :

- le paramétrage du codage des caracteres ::

        define('CHARSET', 'ISO-8859-1');


- le dossier ou sont installées les variables du systeme ::

    define('LOCALE', 'fr_FR');


- Le dossier contenant les locales et les fichiers de traduction ::

    define('LOCALES_DIRECTORY', '../locales');


- Le domaine de traduction ::

    define('DOMAIN', 'openmairie');

Les zones à traduire sont sous le format : _("zone a traduire")

Voir le chapître sur les outils : poEdit



===================================
parametrage de l application metier 
===================================

L'application métier est paramétrée dans *dyn/var.inc*

Ce script contient les parametres globaux de l application . 
Attention les paramètres s'appiquent à toutes les bases de l'application.

Le paramétrage spécifique par collectivité doit se faire dans la table om_parametre 

La configuration générale de l'application se fait aussi dans *dyn/config.inc.php*.

Les paramètres sont récupérés avec la création d'un objet utils par :
$f->config['nom_du_parametre']

Exemple de paramétrage avec openCourrier ::

    $config['application'] = _("openCourrier");
    $config['title'] = ":: "._("openMairie")." :: "._("openCourrier");
    $config['session_name'] = "openCourrier";


* le mode demonstration de l'application se paramétre avec $config['demo']

Ce mode permet de pre-remplir le formulaire de login avec l'identifiant 'demo' et le mot de passe 'demo' ::

    $config['demo'] = false; // true
 
Attention, pour empêcher de changer le mot de passe, il faut paramétrer l'accès
dans la table om_droit : password

* La configuration des extensions autorisees dans le module upload.php

 Pour ajouter votre configuration, decommenter la ligne et modifier les extensions avec des ; comme separateur ::

    $config['upload_extension'] = ".gif;.jpg;.jpeg;.png;.txt;.pdf;.csv;"

* Le theme de l'application - les differents choix possibles se trouvent dans le

  dossier : ../lib/jquery-ui/css/
 
 
  Par defaut, le theme d'openExemple est "om_overcast" ::
  
    $config['theme'] = "om_overcast";


les themes open mairie exemples sont : "om_overcast"; "om_sunny"; "om_ui-darkness";

Vous pouvez mettre d'autres themes jquery.


  
==========================  
Parametrage des librairies
==========================

Le paramétrage de l'accès aux librairies se fait dans *dyn/include.inc.php*

 Ce fichier permet de configurer quels paths vont etre ajoutes a la
 directive include_path du fichier php.ini

 Ce tableau permet de stocker la liste des chemins a ajouter a la directive
 include_path, vous pouvez modifier ces chemins avec vos propres valeurs si
 vos chemins ne sont pas deja inclus dans votre installation, par contre si
 vous avez deja configurer ces chemins dans votre installation vous pouvez
 commenter les lignes suivantes
 
  PEAR ::
  
        array_push($include, getcwd()."/../php/pear");

  DB ::
  
        array_push($include, getcwd()."/../php/db");

  FPDF ::
  
        array_push($include, getcwd()."/../php/fpdf");

  OPENMAIRIE ::

        define("PATH_OPENMAIRIE", getcwd()."/../php/openmairie/");

Par défaut, les librairies sont incluses dans openmairie_exemple :

- /lib : contient les librairies javascript

- /php : contient les librairies php



==========
mode debug
==========

Le mode debug d'openMairie se paramétre dans  *dyn/debug.inc.php*

Ce fichier contient le parametrage pour le mode debug
d'openMairie (om_debug.inc.php)

Valeur de la variable globale DEBUG

  VERBOSE_MODE

  DEBUG_MODE : mode debug

  PRODUCTION_MODE : mode de production (pas de message)
   
=======
version
=======

Vous devez mettre le numéro de version et la date  de votre application
dans *dyn/version.inc*


Voir le versionage des applications dans le guide du développeur.



======================
informations generales
======================


les fichiers textes d'information generale sont à la racine du site

README.txt :

    liste des auteurs ayant participé au projet


HISTORY.txt : information sur chaque version :

            les (+) et les (bugs) corrigés


SPECIFIC.txt :

    Ici, vous décrivez la specificite de l application courante par rapport au framework


LICENCE.txt : licence libre de l application

TODO.txt : feuille de route - roadmap

INSTALL.txt : installation de l application


============
installation
============

La mise en place d une installation automatique est prévue dans la version openMairie 4.0.1


=====================
Les paramétres combos
=====================

Les paramétres combos sont paramétrés dans les fichiers suivants ::

    - comboaffichage.inc.php
    - comboparametre.inc.php
    - comboretour.inc.php

Voir chapître formulaire, sous programme générique combo.php

=======================
Les paramétres éditions
=======================

Les variables dans les éditions sont paramétrées dans ::

    - varpdf.inc                pour les pdf
    - varetatpdf.inc            pour les états et les sous états
    - varlettretypepdf.inc      pour les lettres type
    
Voir chapître édition