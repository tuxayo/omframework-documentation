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
    dyn/var_sig.inc                paramétrage sig 
    dyn/form_sig_update.inc.php    parametrage sig 
    dyn/form_sig_delete.inc.php    parametrage sig 
    
    README.txt                     fichiers textes
    HISTORY.txt
    LICENCE.txt
    TODO.txt
    INSTALL.txt
    
    app/SPECIFIC.txt                explication sur la partie spécifique de l application



==================================
La connexion de la base de donnees
==================================

Le paramétrage de la connexion se fait dans : *dyn/database.inc.php*

Le paramétrage par défaut est dans le tableau $conn[1] pour la base 1 : 

Il peut être paramétré plusieurs bases : conn[1] , conn[2] ...

conn[1] est un tableau php qui contient les paramètres de connexion suivants ::

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
dans openMairie dans sa version actuelle.


=======================
Les zones de navigation
=======================

Quatre zones de navigation différentes sont disponibles dans le framework :

.. image:: ../_static/framework-navigation.png 
    :align: center
    :width: 800px


*******
Le menu
*******

Le but de cette zone de navigation est de rassembler les liens vers toutes les
fonctions du logiciel regroupées par rubrique et catégorie. Elle se situe à
gauche du contenu et est visible uniquement lorsque l'utilisateur est
authentifié. 

.. image:: ../_static/framework-navigation-menu.png 
    :align: center

Par défaut **le menu** est composé de la manière suivante ::


    application             vide par défaut, contient l'accès à votre application
    export                  contient le script "edition" qui reprend
                                les éditions pdf des tables
                            contient le menu "reqmo" qui reprend les requêtes
                                mémorisées
    traitement              vide par défaut, cet option contient les scripts de
                                traitements
    parametrage             Cette option contient vos tables de paramétrage
                                fonctionnel. Par défaut il contient le
                                paramétrage des états / sous-états / lettres type 
    administration          Cette option contient les fonctions de configuration
                                de l'administrateur technique. Cela comprend
                                notamment le paramètrage de la collectivité,
                                om_sig  et la gestion des droits d'accès                                


La configuration des liens se fait dans le fichier :file:`dyn/menu.inc.php`.
Ce fichier de paramétrage n'est pas obligatoire. Si il n'existe pas, aucun lien
n'est affiché. Ce fichier de paramétrage doit contenir la déclaration d'un
tableau de tableaux associatifs dans la variable ``$menu``. Chaque tableau
associatif représente une rubrique. Chaque rubrique contient un tableau de
tableaux associatifs, chacun représentant un lien.

Les caracteristiques de ce tableau sont les suivantes :


    tableau rubrik ::

     title (obligatoire)
     description (texte qui s'affiche au survol de la rubrique)
     href (contenu du lien href)
     class (classe css qui s'affiche sur la rubrique)
     right (droit que l'utilisateur doit avoir pour visionner cette rubrique)
     links (obligatoire)
     open (critères de pré-ouverture de cette rubrique du menu)

    tableau links ::

     title (obligatoire) 
     href (obligatoire) (contenu du lien href)
     class (classe css qui s'affiche sur l'element)
     right (droit que l'utilisateur doit avoir pour visionner cet element)
     target (pour ouvrir le lien dans une nouvelle fenetre)
     open (critères de pré-ouverture de la rubrique du menu dans laquelle est ce
           lien, et sélection de ce lien en lien actif)

L'entrée ``open`` sert à marquer une entrée de menu comme active. La rubrique
contenant cette entrée est ouverte dès l'affichage de la page, et l'entrée active
est mise en évidence. L'entrée ``open`` peut contenir soit une chaîne soit un
``array()`` comportant plusieurs chaînes. Chaque chaîne est créée selon la syntaxe
``'script.php|obj'``, chacune des deux parties étant optionnelle. Le caractère
séparateur ``|`` est obligatoire.

Exemple : ``'\|om_collectivite'`` sélectionnera l'entrée pour toutes les url
          ayant ``obj=om_collectivite``
          
          ``'tab.php|om_collectivite'`` sélectionnera l'entrée pour l'affichage
          du tableau de la classe ``om_collectivite``
          
          ``'unecran.php|'`` sélectionnera l'entrée dès lors que le script
          ``unecran.php`` est appelé quelque soit la classe ``obj``

************************
Les actions personnelles
************************

Le but de cette zone de navigation est de regrouper des liens vers des fonctions
qui concernent les informations de connexion de l'utilisateur. Elle se situe
dans le coin en haut à droite de l'écran et est visible uniquement lorsque
l'utilisateur est authentifié. 

.. image:: ../_static/framework-navigation-actions.png 
    :align: center

Par défaut **les actions personnelles** sont composées de quatre éléments :

* le login de l'utilisateur,
* le libellé de la collectivité,
* un lien vers la page de modification du mot de passe,
* un lien vers la page de déconnexion du logiciel.

Le login de l'utilisateur est récupéré par la méthode ``displayActionLogin()``
de la classe ``om_application``. Cette méthode peut être surchargée dans la
classe ``utils``.

Le libellé de la collectivité est récupéré par la méthode
``displayActionCollectivite()`` de la classe ``om_application``. Cette méthode
peut être surchargée dans la classe ``utils``.

La configuration des liens se fait dans le fichier :file:`dyn/actions.inc.php`.
Ce fichier de paramétrage n'est pas obligatoire. Si il n'existe pas, aucun lien
n'est affiché. Ce fichier de paramétrage doit contenir la déclaration d'un
tableau de tableaux associatifs dans la variable ``$actions``. Chaque tableau
associatif représente un lien.

.. code-block:: php

   <?php
   //
   $actions = array();
   //
   $actions[] = array(
       "title" => _("Link"),
       "description" => _("Description"),
       "href" => "../scr/link.php",
       "target" => "_blank",
       "class" => "action-link",
       "right" => "link",
   );
   ?>

Description de chaque paramètre du tableau associatif :

+-------------+------------+---------------------------------------------------+
| Paramètre   | Requis ?   | Description                                       |
+=============+============+===================================================+
| title       | O          | Texte                                             |
+-------------+------------+---------------------------------------------------+
| description | N          | Texte qui s'affiche au survol de l'élément        |
+-------------+------------+---------------------------------------------------+
| href        | N          | Contenu du lien href                              |
+-------------+------------+---------------------------------------------------+
| target      | N          | Attribut pour ouvrir le lien dans une nouvelle    |
|             |            | fenêtre                                           |
+-------------+------------+---------------------------------------------------+
| class       | N          | Classe CSS qui s'affiche sur l'élément            |
+-------------+------------+---------------------------------------------------+
| right       | N          | Permission nécessaire à l'utilisateur pour        |
|             |            | visualiser l'élément                              |
+-------------+------------+---------------------------------------------------+


**************
Les raccourcis
**************

Le but de cette zone de navigation est de regrouper des liens vers des fonctions
précises utilisées très souvent. Elle se situe en haut à droite de l'écran
juste au dessous des actions personnelles et est visible uniquement lorsque
l'utilisateur est authentifié. 

.. image:: ../_static/framework-navigation-shortlinks.png 
    :align: center

Par défaut **les raccourcis** contiennent uniquement un lien vers le tableau de
bord.

La configuration des liens se fait dans le fichier :file:`dyn/shortlinks.inc.php`.
Ce fichier de paramétrage n'est pas obligatoire. Si il n'existe pas, aucun lien
n'est affiché. Ce fichier de paramétrage doit contenir la déclaration d'un
tableau de tableaux associatifs dans la variable ``$shortlinks``. Chaque tableau
associatif représente un lien.

.. code-block:: php
 
   <?php
   // On initialise le tableau conteneur
   $shortlinks = array();
   // On ajoute au tableau conteneur un tableau associatif représentant un lien
   // (à répéter autant de fois que nécessaire)
   $shortlinks[] = array(
       "title" => _("Link"),
       "description" => _("Description"),
       "href" => "../scr/link.php",
       "target" => "_blank",
       "class" => "action-link",
       "right" => "link",
   );
   ?>

+-------------+------------+---------------------------------------------------+
| Paramètre   | Requis ?   | Description                                       |
+=============+============+===================================================+
| title       | O          | Texte                                             |
+-------------+------------+---------------------------------------------------+
| description | N          | Texte qui s'affiche au survol de l'élément        |
+-------------+------------+---------------------------------------------------+
| href        | N          | Contenu du lien href                              |
+-------------+------------+---------------------------------------------------+
| target      | N          | Attribut pour ouvrir le lien dans une nouvelle    |
|             |            | fenêtre                                           |
+-------------+------------+---------------------------------------------------+
| class       | N          | Classe CSS qui s'affiche sur l'élément            |
+-------------+------------+---------------------------------------------------+
| right       | N          | Permission nécessaire à l'utilisateur pour        |
|             |            | visualiser l'élément                              |
+-------------+------------+---------------------------------------------------+


********************
Les actions globales
********************

Le but de cette zone de navigation est de représenter la section "À propos" du
logiciel. Elle se situe en bas de l'écran juste au dessous du contenu de la
page et est visible lorsque l'utilisateur est authentifié ou non. 

.. image:: ../_static/framework-navigation-footer.png 
    :align: center

Par défaut **les actions globales** sont composées de trois éléments :

* le nom du logiciel ainsi que son numéro de version,
* un lien vers la documentation du site openMairie,
* un lien vers le site openMairie.

Le nom du logiciel est récupéré de la variable ``$config['application']``
présente dans le fichier :file:`dyn/config.inc.php`. La version est récupérée de la
variable ``$version`` présente dans le fichier :file:`dyn/version.inc.php`.

La configuration des liens se fait dans le fichier :file:`dyn/footer.inc.php`.
Ce fichier de paramétrage n'est pas obligatoire. Si il n'existe pas, aucun lien
n'est affiché. Ce fichier de paramétrage doit contenir la déclaration d'un
tableau de tableaux associatifs dans la variable ``$footer``. Chaque tableau
associatif représente un lien.

.. code-block:: php
 
   <?php
   // On initialise le tableau conteneur
   $footer = array();
   // On ajoute au tableau conteneur un tableau associatif représentant un lien
   // (à répéter autant de fois que nécessaire)
   $footer[] = array(
       "title" => _("Link"),
       "description" => _("Description"),
       "href" => "../scr/link.php",
       "target" => "_blank",
       "class" => "action-link",
       "right" => "link",
   );
   ?>

+-------------+------------+---------------------------------------------------+
| Paramètre   | Requis ?   | Description                                       |
+=============+============+===================================================+
| title       | O          | Texte                                             |
+-------------+------------+---------------------------------------------------+
| description | N          | Texte qui s'affiche au survol de l'élément        |
+-------------+------------+---------------------------------------------------+
| href        | N          | Contenu du lien href                              |
+-------------+------------+---------------------------------------------------+
| target      | N          | Attribut pour ouvrir le lien dans une nouvelle    |
|             |            | fenêtre                                           |
+-------------+------------+---------------------------------------------------+
| class       | N          | Classe CSS qui s'affiche sur l'élément            |
+-------------+------------+---------------------------------------------------+
| right       | N          | Permission nécessaire à l'utilisateur pour        |
|             |            | visualiser l'élément                              |
+-------------+------------+---------------------------------------------------+


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

    "DEPRECATED"
    
        define('CHARSET', 'ISO-8859-1');
        ou
        define('CHARSET', 'UTF8');
        
    Dans la version 4.2.0, il y a 2 paramètres :
    
        pour la base : DB_CHARSET
        pour apache  : HTTP_CHARSET
        
        Ces 2 paramètres remplacent CHARSET
    

    Note ::
    
        Dans apache, il est possible de modifiet l'encodage 
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


* La configuration de la taille maximale des fichiers dans le module upload.php

Pour changer votre configuration, décommenter la ligne et modifier la taille. La taille maximale est en mo. ::

    $config['upload_taille_max'] = str_replace('M', '', ini_get('upload_max_filesize')) * 1024;


* Le thème de l'application

A partir de la version 3.1.0, le theme n'est plus géré dans config.inc.php.
Il est initialisé dans EXTERNALS.TXT du repertoire om-theme (version 4.2.0) ::

    exemple pour om_ui_darkness 
    
    om_theme svn://scm.adullact.net/svnroot/openmairie/externals/jquery-ui-theme/
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

  OPENMAIRIE (dans CORE depuis la version 4.2.0) ::

        define("PATH_OPENMAIRIE", getcwd()."../core/openmairie/"); 


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
  $this->addToLog("requete sig_interne maj parcelle inexistante :".$sql, EXTRA_VERBOSE_MODE);

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

les paramètres sont les suivants ::

    $contenu_etendue[0]= array('4.5868,43.6518,4.6738,43.7018'
                              );
    $contenu_etendue[1]= array('vitrolles'
                              );
    $contenu_epsg[0] = array("","EPSG:2154","EPSG:27563");
    $contenu_epsg[1] = array("choisir la projection",'lambert93','lambertSud');
    $type_geometrie[0] = array("","point","line","polygon");
    $type_geometrie[1] = array("choisir le type de géométrie",'point','ligne','polygone');

ces paramétres sont utilisés pour la saisie de carte : voir chapître sig

Les post traitements de form_sig permettent de faire des traitement apres saisie de géométries avec om_sig


    form_sig_update.inc.php

    form_sig_delete.inc.php

exemple recuperation du numéro de la parcelle dans openfoncier  dossier ::

    if($table=="dossier" and $champ=="geom"){
       echo "</center>";
       if (file_exists ("../dyn/var.inc"))
          include ("../dyn/var.inc");
       // parcelle         
       if($auto_parcelle==1){
          $sql="select parcelle from ".DB_PREFIXE."parcelle  WHERE
            ST_contains(geom,  geometryfromtext('".$geom."', ".$projection."))";
          $parcelle = $f->db -> getOne($sql);
          if($parcelle!=''){
             $sql ="update ".DB_PREFIXE."dossier set parcelle = '".$parcelle."'
                where dossier = '".$idx."'";
             $res1 =  $f->db -> query($sql);
             echo "<br>"._("parcelle")." ".$parcelle;
             // Envoi des donnees dans le formulaire f1 si la fenetre est popup
             if($popup==1){
                echo "\n<script type=\"text/javascript\">\n";
                echo "window.opener.fendata.document.f1.parcelle.value = '".$parcelle."';\n";
                //echo "window.opener.fendata.reload";
                echo "</script>\n";
             }
          }
       }
    ....
