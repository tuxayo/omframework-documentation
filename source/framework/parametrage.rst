.. _parametrage:

###########
Parametrage
###########

Le paramétrage de l application est dans le répertoire /dyn.

Il est proposé dans ce chapitre de décrire les différents fichiers de paramétrage.


===============================
connexion de la base de donnees
===============================

*dyn/database.inc.php*

Le parametrage par defaut est dans le tableau $conn[1] pour la base 1 : 


conn[1] est un tableau php qui contient les parametres de connexion suivants ::

    'titre => 'openxxx (mysql ou pgsql)',[parametrage openmairie]
    'phptype'  => 'mysql', ou 'pgsql' [ne pas changer parametrage dbpear]
    'dbsyntax' => '',[ne pas changer parametrage dbpear]
    'username' => 'root', [par defaut sur wamp easyphp ou lamp /
                           a voir avec le fournisseur d acces le cas echeant]
    'password' => '' [par defaut sur wamp easyphp ou lamp /
                        a voir avec le fournisseur d acces le cas echeant]                     
    'protocol' => '',
    'hostspec' => 'localhost', [nom de serveur par defaut wamp ou easyphp]
    'port'     => '',  [ne pas changer parametrage dbpear]
    'socket'   => '',  [ne pas changer parametrage dbpear]
    'nom de la base' => 'openxxx', [parametrage openmairie]
    'format date' par defaut =>'AAAA-MM-JJ' [[parametrage openmairie ne pas changer]
    'shema' => '' ou 'public' pour postgre
    'prefixe' => '' 



==============
menu principal
==============

*dyn/menu.inc.php*

$menu est le tableau associatif qui contient tout le menu de l'application,
il contient lui meme un tableau par rubrique, puis chaque
rubrique contient un tableau par lien :

les caracteristiques sont les suivantes :

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

*dyn/action.inc*

$actions est le tableau associatif qui contient tous les liens presents dans
les actions a cote du login et du nom de la collectivite

les caracteristiques sont les suivantes :


- tableau link ::

    title (obligatoire)
    description (texte qui s'affiche au survol de l'element)
    href (obligatoire) (contenu du lien href)
    class (classe css qui s'affiche sur l'element)
    right (droit que l'utilisateur doit avoir pour visionner cet element)
    target (pour ouvrir le lien dans une nouvelle fenetre)

*dyn/shortlinks.inc*

Ce fichier permet de configurer les liens presents dans la barre de
raccourcis presente en dessous des actions

$shortlinks est le tableau associatif qui contient tous les liens presents
dans les raccourcis en dessous des actions
 
les caracteristiques sont les suivantes :


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

*dyn/tdb.inc* 

    Le parametrage est libre et depend de l application. 

**Exemple** ::

    $description = _("Bienvenue ").$_SESSION["login"]."<br>";    
    $f->displayDescription($description);


================
locales : langue
================

*dyn/locales.inc.php*

Ce fichier contient :

- Codage des caracteres ::

        define('CHARSET', 'ISO-8859-1');


- Pour voir les autres locales disponibles, il faut voir le contenu du dossier locales/ et il faut que cette locale soit installee sur votre systeme ::

    define('LOCALE', 'fr_FR');


- Le dossier contenant les locales et les fichiers de traduction ::

    define('LOCALES_DIRECTORY', '../locales');


- Le domaine de traduction ::

    define('DOMAIN', 'openmairie');

Les zones à traduire sont sous le format : _("zone a traduire")

Voir outil / poEdit


===================================
parametrage de l application metier 
===================================

*dyn/var.inc*

Ce script contient les parametres globaux de l application . 
(toutes bases et toutes collectivités confondues)

Le paramétrage par collectivité se fait dans la table om_parametre 

*dyn/config.inc.php*

Exemple openCourrier ::

    $config['application'] = _("openCourrier");
    $config['title'] = ":: "._("openMairie")." :: "._("openCourrier");
    $config['session_name'] = "openCourrier";


* Mode demonstration de l'application

Il permet de pre-remplir le formulaire de login avec l'identifiant 'demo' et le mot de passe 'demo' ::

    $config['demo'] = false; // true
    


* Configuration des extensions autorisees dans le module upload.php

 Pour ajouter votre configuration, decommenter la ligne et modifier les extensions avec des ; comme separateur ::

    $config['upload_extension'] = ".gif;.jpg;.jpeg;.png;.txt;.pdf;.csv;"

* Theme de l'application - les differents choix possibles se trouvent dans le

  dossier : ../lib/jquery-ui/css/
 
 
  Default ::
  
    $config['theme'] = "om_overcast";


les themes open mairie exemples sont : "om_overcast"; "om_sunny"; "om_ui-darkness";

Vous pouvez mettre d'autres themes jquery.


  
==========================  
Parametrage des librairies
==========================

*dyn/include.inc.php*

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

==========
mode debug
==========

*dyn/debug.inc.php*

Ce fichier contient le parametrage pour le mode debug
d'openMairie (om_debug.inc.php)

Valeur de la variable globale DEBUG

  VERBOSE_MODE

  DEBUG_MODE : mode debug

  PRODUCTION_MODE : mode de production (pas de message)
   
=======
version
=======

*dyn/version.inc*

ce fichier contient la date et numero de la version courante

======================
informations generales
======================


les fichiers textes d'information generale sont a la racine du site

README.txt :

    liste des auteurs ayant participé au projet


HISTORY.txt : information sur chaque version :

            les (+) et les (bugs) corrigés


SPECIFIC.txt :

    description de la specificite de l application courante / framework


LICENCE.txt : licence libre de l application

TODO.txt : feuille de route - roadmap

INSTALL.txt : installation de l application

============
installation
============

La mise en place d une installation automatique est prévue dans la version openMairie 4.0.1