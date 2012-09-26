.. _parametrage_generateur:

======================
Paramétrage générateur
======================

Le paramétrage de base est dans la classe gen. Il est possible de personnaliser
le paramétrage dans le répertoire gen/dyn. (version 4.2.0)

Ne personnaliser que les variables souhaitées dans le fichier. Par défaut,
openMairie prendra les paramètres inclus dans la classe gen.

Il est donné ci dessous l ensemble des paramètres personnalisables. La valeur
associée est celle du générateur.


``gen/dyn/gen.inc.php``
=======================

Il permet de définir des paramètres généraux pouvant être utilisés partout dans
le générateur.

.. code-block:: php
 
   <?php
   /***
    * Mode de génération pour la gestion des identifiants et des références
    *
    * Permet de choisir par quel moyen sont récupérées les clés primaires et les
    * clés étrangères :
    *  - "constraints" => en interrogeant les contraintes de la base de données
    *                     (fonctionne uniquement avec PostGreSQL)
    *  - "postulate" => par les postulats :
    *    _ "le nom d'un champ 'clé primaire' a pour nom le nom de la table."
    *    _ "le nom d'un champ 'clé étrangère' a pour nom le nom de la table vers
    *      laquelle elle fait référence, et fait référence au champ clé primaire
    *      de cette table."
    * 
    * Default : $key_constraints_mode = "constraints";
    ***/
   if (OM_DB_PHPTYPE == "mysql") {
       $key_constraints_mode = "postulate";
   }
   ?>

.. code-block:: php
 
   <?php
   /***
    * Liste des tables à ne pas générer
    *
    * Permet de lister les tables dont la génération n'est pas souhaitable. Ces
    * tables n'apparaissent donc plus dans le menu de génération ni dans la
    * génération complète.
    * 
    * Default : $tables_to_avoid = array();
    ***/
   $tables_to_avoid = array(
       "om_version",
       "spatial_ref_sys",
   );
   ?>


``gen/dyn/tab.inc.php``
=======================

Ce fichier de paramétrage permet de lister pour chaque table la liste des
colonnes à positionner dans la variable ``$champAffiche`` lors de la génération
des fichiers ``gen/sql/<OM_DB_PHPTYPE>/<TAB>.inc.php``.

.. code-block:: php
 
   <?php
   // Table om_utilisateur
   $om_utilisateur = array("nom", "email", "login", "om_profil", );
   ?>


Il est inutile de faire apparaitre ici la colonne portant la contrainte
``PRIMARY KEY``. Cette colonne est obligatoire pour le bon fonctionnement du
framework.

Il est inutile de faire apparaitre ici la colonne ``om_collectivite``. Cette
colonne apparaîtra d'office si la colonne est dans la table.


``gen/dyn/form.inc.php``
========================

Voici les paramètres pour la génération de formulaire ::

    $serie = 15;                        nombre d'enregistrement par page'
    $ico ="../img/ico_application.png"; icone DEPRECATED 
    $max=6;                             nb de ligne blob
    $taille=80;                         taille du blob
    $pgsql_longueur_date=12;            taille d'affichage de la date '
    
    *** deprecated
    $pgsql_taille_defaut = 20;          taille du champ par defaut si retour pg_field_prtlen =0
    $pgsql_taille_minimum    = 10;      taille minimum d affichage d un champ
    ***/ 


``gen/dyn/pdf.inc.php``
=======================

Parametres ::

    $longueurtableau= 280;
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


``gen/dyn/etat.inc.php``
========================

parametres ::

    $variable='&'; // nouveau
    // parametres
    $etat['orientation']='P';
    $etat['format']='A4';
    // footer
    $etat['footerfont']='helvetica';
    $etat['footerattribut']='I';
    $etat['footertaille']='8';
    // logo
    $etat['logo']='logopdf.png';
    $etat['logoleft']='58';
    $etat['logotop']='7';
    // titre
    $etat['titreleft']='41';
    $etat['titretop']='36';
    $etat['titrelargeur']='130';
    $etat['titrehauteur']='10';
    $etat['titrefont']='helvetica';
    $etat['titreattribut']='B';
    $etat['titretaille']='15';
    $etat['titrebordure']='0';
    $etat['titrealign']='C'; 
    // corps
    $etat['corpsleft']='7';
    $etat['corpstop']='57';
    $etat['corpslargeur']='195';
    $etat['corpshauteur']='5';
    $etat['corpsfont']='helvetica';
    $etat['corpsattribut']='';
    $etat['corpstaille']='10';
    $etat['corpsbordure']='0';
    $etat['corpsalign']='J';
    // sous etat
    $etat['se_font']='helvetica';
    $etat['se_margeleft']='8';
    $etat['se_margetop']='5';
    $etat['se_margeright']='5';
    $etat['se_couleurtexte']="0-0-0";


``gen/dyn/sousetat.inc.php``
============================

parametres::

    $longueurtableau= 195;
    $variable='&'; // nouveau
    // parametres
    
    //titre
    $sousetat['titrehauteur']=10;
    $sousetat['titrefont']='helvetica';
    $sousetat['titreattribut']='B';
    $sousetat['titretaille']=10;
    $sousetat['titrebordure']=0;
    $sousetat['titrealign']='L';
    $sousetat['titrefond']=0;
    $sousetat['titrefondcouleur']="255-255-255";
    $sousetat['titretextecouleur']="0-0-0";
    // intervalle
    $sousetat['intervalle_debut']=0;
    $sousetat['intervalle_fin']=5;
    // entete
    $sousetat['entete_flag']=1;
    $sousetat['entete_fond']=1;
    $sousetat['entete_hauteur']=7;
    $sousetat['entete_fondcouleur']="255-255-255";
    $sousetat['entete_textecouleur']="0-0-0";
    // tableau
    $sousetat['tableau_bordure']=1;
    $sousetat['tableau_fontaille']=10;
    // bordure
    $sousetat['bordure_couleur']="0-0-0";
    // sous etat fond
    $sousetat['se_fond1']="243-246-246";
    $sousetat['se_fond2']="255-255-255";
    // cellule
    $sousetat['cellule_fond']=1;
    $sousetat['cellule_hauteur']=7;
    // total
    $sousetat['cellule_fond_total']=1;
    $sousetat['cellule_fontaille_total']=10;
    $sousetat['cellule_hauteur_total']=15;
    $sousetat['cellule_fondcouleur_total']="255-255-255";
    // moyenne
    $sousetat['cellule_fond_moyenne']=1;
    $sousetat['cellule_fontaille_moyenne']=10;
    $sousetat['cellule_hauteur_moyenne']=5;
    $sousetat['cellule_fondcouleur_moyenne']="212-219-220";
    // nombre d enregistrement
    $sousetat['cellule_fond_nbr']=1;
    $sousetat['cellule_fontaille_nbr']=10;
    $sousetat['cellule_hauteur_nbr']=7;
    $sousetat['cellule_fondcouleur_nbr']="255-255-255";


``gen/dyn/lettretype.inc.php``
==============================

parametres ::

    // general
    $variable='&'; // nouveau
    // $variable=chr(163); // compatibilite openmairie <4
    // parametres
    $lettretype['orientation']='P';
    $lettretype['format']='A4';
    // logo
    $lettretype['logo']='logopdf.png';
    $lettretype['logoleft']='58';
    $lettretype['logotop']='7';
    // titre
    $lettretype['titreleft']='41';
    $lettretype['titretop']='36';
    $lettretype['titrelargeur']='130';
    $lettretype['titrehauteur']='10';
    $lettretype['titrefont']='helvetica';
    $lettretype['titreattribut']='B';
    $lettretype['titretaille']='15';
    $lettretype['titrebordure']='0';
    $lettretype['titrealign']='C'; 
    // corps
    $lettretype['corpsleft']='7';
    $lettretype['corpstop']='57';
    $lettretype['corpslargeur']='195';
    $lettretype['corpshauteur']='5';
    $lettretype['corpsfont']='helvetica';
    $lettretype['corpsattribut']='';
    $lettretype['corpstaille']='10';
    $lettretype['corpsbordure']='0';
    $lettretype['corpsalign']='J';
