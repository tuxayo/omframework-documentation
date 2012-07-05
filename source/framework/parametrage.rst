.. _parametrage:

########################
Paramétrage du framework
########################

Le paramétrage de l'application se fait dans le répertoire /dyn.

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
    dyn/debug.inc.php              mode debug
    dyn/version.inc                paramétrage de la version
    
    README.txt                     fichiers textes
    HISTORY.txt
    SPECIFIC.txt
    LICENCE.txt
    TODO.txt
    INSTALL.txt
    



==================================
La connexion de la base de donnees
==================================

Le paramétrage de la connexion se fait dans : *dyn/database.inc.php*

Le paramétrage par défaut est dans le tableau $conn[1] pour la base 1 : 

Il peut être paramétré plusieurs bases : conn[1] , conn[2] ...

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
dans openMairie dans sa version actuelle


=================
Le menu principal
=================

Le paramétrage du menu se fait dans le fichier *dyn/menu.inc.php*.

De base, les rubriques suivantes sont paramétrées dans le framework::

    application             vide par défaut, contient l'accès à votre application
    export                  contient le script "edition" qui reprend
                                les éditions pdf des tables
                            contient le menu "reqmo" qui reprend les requêtes
                                mémorisées
    traitement              vide par défaut, cet option contient les scripts de
                                traitement
    parametrage             Cette option contient vos tables de paramétrage
    administration          Les scripts de cet option contiennent tout les scripts
                                du framework pour le paramètrage de la collectivité,
                                des états / sous états  et la gestion des accès                                

Le paramétrage du menu se fait dans $menu.

$menu est le tableau associatif qui contient tout le menu de l'application,
il contient lui meme un tableau par rubrique, puis chaque
rubrique contient un tableau par lien :

Les caracteristiques de ce tableau sont les suivantes :


    tableau rubrik ::

     title (obligatoire)
     description (texte qui s'affiche au survol de la rubrique)
     href (contenu du lien href)
     class (classe css qui s'affiche sur la rubrique)
     right (droit que l'utilisateur doit avoir pour visionner cette rubrique)
     links (obligatoire)

    tableau links ::

     title (obligatoire) 
     href (obligatoire) (contenu du lien href)
     class (classe css qui s'affiche sur l'element)
     right (droit que l'utilisateur doit avoir pour visionner cet element)
     target (pour ouvrir le lien dans une nouvelle fenetre)


============
Le menu haut
============

Le paramétrage du menu haut se fait dans le fichier *dyn/action.inc.php*

Par défaut, il est paramétré le changement de mot de poste et la déconnexion


$actions est le tableau associatif qui contient tous les liens présents dans
les actions à côté du login et du nom de la collectivite

les caractéristiques du tableau link sont les suivantes :


tableau link ::


    title (obligatoire)
    description (texte qui s'affiche au survol de l'element)
    href (obligatoire) (contenu du lien href)
    class (classe css qui s'affiche sur l'element)
    right (droit que l'utilisateur doit avoir pour visionner cet element)
    target (pour ouvrir le lien dans une nouvelle fenetre)

Les liens sous le menu des actions se paramétrent dans le fichier : *dyn/shortlinks.inc.php*

$shortlinks est le tableau associatif qui contient tous les liens présents
dans les raccourcis qui se situent en dessous des actions du menu haut
 
Par défaut, il est paramétré l'accès au tableau de bord.

Les caracteristiques du tableau $link sont les suivantes :


tableau link ::

    title [obligatoire]
    description (texte qui s'affiche au survol de l'element)
    href [obligatoire] (contenu du lien href)
    class (classe css qui s'affiche sur l'element)
    right (droit que l'utilisateur doit avoir pour visionner cet element)
    target (pour ouvrir le lien dans une nouvelle fenetre)


==================
Le tableau de bord
==================

Le tableau de bord se paramètre dans le fichier *dyn/dashboard.inc*. 

Ce fichier est appellé par le script scr/dashboard.php.

Pour avoir son propre tableau de bord, il suffit de decommenter la ligne 
// die(); et on accède plus au widget

Voir chapître : widget et tableau de bord paramétrable


==================================
Les variables locales et la langue
==================================

Les variables locales sont paramétrées dans le fichier *dyn/locales.inc.php*

Ce fichier contient :


- le paramétrage du codage des caracteres (ISO-8859-1 ou UTF8)  ::

        define('CHARSET', 'ISO-8859-1');
        ou
        define('CHARSET', 'UTF8');

    Verifier le paramétrage d'APACHE si il y a un mauvais affichage par défaut ISO-8859-1
    dans etc/apache2/apache2.conf commenter ##AddDefaultCharset = ISO-8859-1
    relancer ensuite apache : $ etc/apache2/init.d/apache2 reload
    
    A partir de la version 3.0.1, l'imcompatibilité utf8 de la bibliotheque fpdf est traitée

- le dossier ou sont installées les variables du systeme ::

    define('LOCALE', 'fr_FR');


- Le dossier contenant les locales et les fichiers de traduction ::

    define('LOCALES_DIRECTORY', '../locales');


- Le domaine de traduction ::

    define('DOMAIN', 'openmairie');

Les zones à traduire sont sous le format : _("zone a traduire")


Voir le chapître sur les outils : *poEdit*



======================================
Le paramétrage de l application metier 
======================================

L'application métier est paramétrée dans *dyn/var.inc*

Ce script contient les paramétres globaux de l application . 
Attention les paramètres s'appliquent à toutes les bases de l'application.

Le paramétrage spécifique par collectivité doit se faire dans la table om_parametre 

La configuration générale de l'application se fait aussi dans *dyn/config.inc.php*.

Les paramètres sont récupérés avec la création d'un objet utils par :
$f->config['nom_du_parametre']

*Voir framework/utilitaire*

Exemple de paramétrage avec openCourrier ::

    $config['application'] = _("openCourrier");
    $config['title'] = ":: "._("openMairie")." :: "._("openCourrier");
    $config['session_name'] = "openCourrier";


* le mode demonstration de l'application se paramétre avec $config['demo']

Ce mode permet de pre-remplir le formulaire de login avec l'identifiant 'demo' et le mot de passe 'demo' ::

    $config['demo'] = false;  l'application n'est pas en mode démo
                      true; l'application est en mode démo
 
    Attention, pour empêcher de changer le mot de passe, il faut paramétrer l'accès
    dans la table om_droit : password


* La configuration des extensions autorisees dans le module upload.php

 Pour changer votre configuration, décommenter la ligne et modifier les extensions avec des ";" comme séparateur ::

    $config['upload_extension'] = ".gif;.jpg;.jpeg;.png;.txt;.pdf;.csv;"


* Le thème de l'application

A partir de la version 3.1.0, le theme n'est plus géré dans config.inc.php.
Il est initialisé dans EXTERNALS.TXT du repertoire lib/om-theme ::

    exemple pour om_ui_darkness 
    
    jquery-ui-theme svn://scm.adullact.net/svnroot/openmairie/externals/jquery-ui-theme/
                    om_ui-darkness/tags/1.8.14


  
=============================  
Le Parametrage des librairies
=============================

Le paramétrage de l'accès aux librairies se fait dans *dyn/include.inc.php*

 Ce fichier permet de configurer les paths en fonction de la 
 directive include_path du fichier php.ini. 
 Vous pouvez aussi modifier ces chemins avec vos propres valeurs si
 vous voulez personnaliser votre installation :
 
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



=============
Le mode debug
=============

Le mode debug d'openMairie se paramétre dans  *dyn/debug.inc.php*

Ce fichier contient le paramétrage pour le mode debug
d'openMairie (om_debug.inc.php)

Valeur de la variable globale DEBUG ::

  EXTRA_VERBOSE_MODE : mode très bavard qui reprend les messages spécifiques
  dans la méthode addToLog
  exemple :
  $this->addToLog("requete sig_interne maj parcelle inexistante :".$sql, VERBOSE_MODE);

  VERBOSE_MODE : mode "bavard"
  dans ce mode , il est créé un fielset sous les formulaires qui indiquent
  toutes les étapes de réalisation des scripts

  DEBUG_MODE : mode debug
  Les messages d'erreur sont visibles

  PRODUCTION_MODE : mode de production (il n y a pas de message)
   
===============================
La version de votre application
===============================

Vous devez mettre le numéro de version et la date  de votre application
dans *dyn/version.inc*


Voir *le versionage des applications*.



==========================
Les informations generales
==========================


Les fichiers textes d'information générale sont à la racine de l'application  :

README.txt :

    ce fichier peut contenir entre autre, la liste des auteurs ayant participé au projet


HISTORY.txt : information sur chaque version :

            les (+) et les (bugs) corrigés


app/SPECIFIC.txt :

    Ici, vous décrivez la specificite de l application courante par rapport au framework


LICENCE.txt : licence libre de l application

TODO.txt : feuille de route - roadmap

INSTALL.txt : installation de l application


==========================
L'installation automatique
==========================

La mise en place d une installation automatique est prévue dans une prochaine version openMairie.


=========================
Les paramétres des combos
=========================

Les paramétres des combos sont paramétrés dans les fichiers suivants (type de contrôle
de formulaire comboD et comboG (pour formulaire) ou comboD2 et comboG2 (pour sous formulaire) ::

    - comboaffichage.inc.php :
        paramétre de l'affichage dans la fenêtre combo.php
    - comboparametre.inc.php
        affecte des valeus spécifiques au formulaire parent si il y a plusieurs
        enregistrement en lien (choix en affichage)
    - comboretour.inc.php
        meme chose que comboparametre.inc si il n'y a qu un enregistrement en lien
        (pas d'affichage de la fenetre)

Voir *chapître framework/formulaire, sous programme générique combo.php*

=======================
Les paramétres éditions
=======================

Les variables dans les éditions sont paramétrées dans ::

    - varpdf.inc                pour les pdf
    - varetatpdf.inc            pour les états et les sous états
    - varlettretypepdf.inc      pour les lettres type
    
Voir *chapître framework/édition*



=====================
Les paramétres om_sig
=====================

var_sig.php



Les post traitements de form_sig

form_sig_update.inc.php

form_sig_delete.inc.php

