.. _parametrage_generateur:

======================
Paramétrage générateur
======================

Le générateur fonctionne de manière autonome sans fichier de paramétrage. Il
est tout de même possible de paramétrer certains éléments grâce à des fichiers
de paramétrage déposés dans le répertoire ``gen/dyn/``.

Il n'est pas nécessaire de personnaliser toutes les variables du fichier. Il est
recommandé de déclarer uniquement les paramètres souhaitées. Par défaut, le
générateur prend les paramètres inclus dans la classe gen.


``gen/dyn/gen.inc.php``
=======================

Il permet de définir des paramètres généraux pouvant être utilisés partout dans
le générateur.

.. code-block:: php
 
   <?php
   /**
    * Mode de génération pour la gestion des identifiants et des références
    *
    * Permet de choisir par quel moyen sont récupérées les clés primaires et les
    * clés étrangères :
    *  - "constraints" => en interrogeant les contraintes de la base de données
    *  - "postulate" => par les postulats :
    *    _ "le nom d'un champ 'clé primaire' a pour nom le nom de la table."
    *    _ "le nom d'un champ 'clé étrangère' a pour nom le nom de la table vers
    *      laquelle elle fait référence, et fait référence au champ clé primaire
    *      de cette table."
    * 
    * Default : $key_constraints_mode = "constraints";
    */
   $key_constraints_mode = "postulate";
 
 
  
   /**
    * Liste des tables à ne pas générer
    *
    * Permet de lister les tables dont la génération n'est pas souhaitable. Ces
    * tables n'apparaissent donc plus dans le menu de génération ni dans la
    * génération complète.
    * 
    * Default : $tables_to_avoid = array();
    */
   $tables_to_avoid = array(
       "om_version",
       "spatial_ref_sys",
   );
   
   /**
    * Ce tableau de configuration permet de donner des informations de surcharges
    * sur certains objets pour qu'elles soient prises en compte par le générateur.
    *
    * $tables_to_overload = array(
    *    "<table>" => array(
    *        // définition de la liste des classes qui surchargent la classe
    *        // <table> pour que le générateur puisse générer ces surcharges 
    *        // et les inclure dans les tests de sous formulaire
    *        "extended_class" => array("<classe_surcharge_1_de_table>", ),
    *        // définition de la liste des champs à afficher dans l'affichage du
    *        // tableau champAffiche dans <table>.inc.php
    *        "displayed_fields_in_tableinc" => array("<champ_1>", ),
    *        // définition des composantes du titre de la page : 'tablename' est 
    *        // la dernière composante du titre et 'breadcrumb' est la liste des 
    *        // premières composantes. Ce sont les chaines à traduire qui doivent
    *        // être saisies ici. Le résultat serait : 
    *        // $ent = _("<libelle_1>")." -> "._("<libelle_2>")." -> "._("<libelle>");
    *        "breadcrumb_in_page_title" => array("<libelle_1>", "<libelle_2>", ),
    *        "tablename_in_page_title" => "<libelle>",
    *        // désactivation des sousformulaires pour que les onglets ne s'affichent
    *        // pas dans le contexte de la table
    *        "tabs_in_form" => false,
    *        // définition de l'option 'om_validite' pour que les colonnes soient
    *        // cachées par défaut
    *        "om_validite" => "hidden_by_default",
    *    ),
    * );
    */
   $tables_to_overload = array(
       //
       "om_widget" => array(
           //
           "displayed_fields_in_tableinc" => array(
               "libelle", "om_profil", "type", 
           ),
           // 
           "breadcrumb_in_page_title" => array("administration", "tableaux de bord", ),
           "tablename_in_page_title" => "widgets",
           // 
           "tabs_in_form" => false,
           //
           "om_validite" => "hidden_by_default",
       ),
   );
   
   ?>


``gen/dyn/tab.inc.php``
=======================

Ce fichier n'est plus utilisé par le générateur depuis la version 4.5, les paramètres
gérés dans ce dernier ont été transférés dans le fichier ``gen/dyn/gen.inc.php``.


``gen/dyn/form.inc.php``
========================

Ce script permet de personnaliser les éditions générées. On peut par exemple générer 
toutes les éditions au format A3.

Voici les variables personnalisables :

.. code-block:: php

   <?php
   
   /**
    * Nombre d'enregistrements par page dans les listings
    */
   $serie = 15;
   
   /**
    * Icône utilisée auparavant comme lien vers l'aide
    * @deprecated
    */
   $ico = "../img/ico_application.png";
   
   /**
    * Taille d'affichage du champ text (nombre de lignes)
    */
   $max = 6;
   
   /**
    * Taille d'affichage du champ text (nombre de colonnes)
    */
   $taille = 80;
   
   /**
    * Taille d'affichage du champ par défaut dans le cas où nous sommes
    * dans l'impossibilité de déterminer la taille du champ.
    * Uniquement pour le SGBD PostGreSQL
    */
   $pgsql_taille_defaut = 20;
   
   /**
    * Taille d'affichage du champ minimum pour ne pas afficher des
    * champs trop petits où la saisie serait impossible
    * Uniquement pour le SGBD PostGreSQL
    */
   $pgsql_taille_minimum = 10;
   
   /**
    * Taille d'affichage du champ maximum pour ne pas afficher des
    * champs trop grands où le formulaire dépasserait de l'écran
    * Uniquement pour le SGBD PostGreSQL
    */
   $pgsql_taille_maximum = 30;
   
   /**
    * Taille d'affichage de la date
    * Uniquement pour le SGBD PostGreSQL
    */
   $pgsql_longueur_date = 12;
   
   ?>


``gen/dyn/permissions.inc.php``
===============================

Ce script permet de paramétrer la génération des permissions. L'objectif ici est de pouvoir
indiquer des scripts à ne pas examiner et des permissions à ajouter à celles trouvées
automatiquement.

Voici les paramètres disponibles :

.. code-block:: php 
éthode autocomplete ne peut être active que dans formulaire
..
 
   <?php
   
   /**
    * Liste des fichiers à ne pas prendre en compte
    *
    * Permet de lister les fichiers du répertoire obj/ dans lequel le système
    * de génération des permissions ne doit pas passer.
    * 
    * Default : $files_to_avoid = array();
    */
   $files_to_avoid = array(
       "pdf_lettre_rar.class.php",
       "pilotage.class.php
  );
   
   /**
    * Liste des permissions spécifiques
    *
    * Permet de lister les permission que le système de génération des permissions
    * n'est pas en mesure de trouver.
    * 
    * Default : $permissions = array();
    */
   $permissions = array(
       "proces_verbal_fichier_telecharger",
       "dossier_instructeur_modifier_instructeur",
   );
   
   ?>


``gen/dyn/pdf.inc.php``
=======================

Ce script permet de personnaliser les éditions générées. On peut par exemple générer 
toutes les éditions au format A3.

Voici les variables personnalisables :

.. code-block:: php

   <?php
   
   $longueurtableau = 280;
   $orientation='L';// orientation P-> portrait L->paysage";
   $format='A4';// format A3 A4 A5;
   $police='arial';
   $margeleft=10;// marge gauche;
   $margetop=5;// marge haut;
   $margeright=5;//  marge droite;
   $border=1; // 1 ->  bordure 0 -> pas de bordure";
   $C1=0;// couleur texte  R";
   $C2=0;// couleur texte  V";
   $C3=0;// couleur texte  B";
   $size=10; //taille POLICE";
   $height=4.6; // hauteur ligne tableau ";
   $align='L';
   // fond 2 couleurs
   $fond=1;// 0- > FOND transparent 1 -> fond";
   $C1fond1=234;// couleur fond  R ";
   $C2fond1=240;// couleur fond  V ";
   $C3fond1=245;// couleur fond  B ";
   $C1fond2=255;// couleur fond  R";
   $C2fond2=255;// couleur fond  V";
   $C3fond2=255;// couleur fond  B";
   // spe openelec
   $flagsessionliste=0;// 1 - > affichage session liste ou 0 -> pas d'affichage";
   // titre
   $bordertitre=0; // 1 ->  bordure 0 -> pas de bordure";
   $aligntitre='L'; // L,C,R";
   $heightitre=10;// hauteur ligne titre";
   $grastitre='B';//\$gras='B' -> BOLD OU \$gras=''";
   $fondtitre=0; //0- > FOND transparent 1 -> fond";
   $C1titrefond=181;// couleur fond  R";
   $C2titrefond=182;// couleur fond  V";
   $C3titrefond=188;// couleur fond  B";
   $C1titre=75;// couleur texte  R";
   $C2titre=79;// couleur texte  V";
   $C3titre=81;// couleur texte  B";
   $sizetitre=15;
   // entete colonne
   $flag_entete=1;//entete colonne : 0 -> non affichage , 1 -> affichage";
   $fondentete=1;// 0- > FOND transparent 1 -> fond";
   $heightentete=10;//hauteur ligne entete colonne";
   $C1fondentete=210;// couleur fond  R";
   $C2fondentete=216;// couleur fond  V";
   $C3fondentete=249;// couleur fond  B";
   $C1entetetxt=0;// couleur texte R";
   $C2entetetxt=0;// couleur texte V";
   $C3entetetxt=0;// couleur texte B";
   $C1border=159;// couleur texte  R";
   $C2border=160;// couleur texte  V";
   $C3border=167;// couleur texte  B";
   $bt=1;// border 1ere  et derniere ligne  du tableau par page->0 ou 1";
   
   ?>


``gen/dyn/etat.inc.php``
========================

Ce fichier n'est plus utilisé par le générateur depuis la version 4.0 et la gestion des éditions en base de données.


``gen/dyn/sousetat.inc.php``
============================

Ce fichier n'est plus utilisé par le générateur depuis la version 4.0 et la gestion des éditions en base de données.



``Limites du générateur``
========================

Nous décrivons ici les limites du générateur qui pourront être réglées dans des versions ultérieures

Lorsqu'un formulaire principal (FP) contient un champ nommé "libelle" ainsi qu'un sous formulaire (FS) contenant lui-même un champ "libelle", cela pose plusieurs problèmes :

- pas w3c compliant,

- lors de l'exécution de javascript sur le sélecteur (id) du champ, en effet l'id du champ est le nom de la colonne correspondante dans la bdd.

fonctionnement
La solution de contournement exemple avec autocomplete ayant le même nom en form et sous form 

1- en modification désactiver les onglets : $option_tab_disabled_on_edit=true;
la méthode autocomplete ne peut être active que dans formulaire

2- surcharge de la méthode autocomplete dans obj/om_formulaire.class.php 

.. code-block :: php

    if($this->correct){
       $this->text($champ, $validation, $DEBUG = false);
    }else{
    ...
    
si la validation est ok, on n utilise plus la methode autocomplete dans le formulaire de retour qui gene l'utilisation de cette mếme méthode dans le sous fomulaire
