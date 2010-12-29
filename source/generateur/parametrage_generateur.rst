.. _parametrage_generateur:

######################
Paramétrage générateur
######################

Le paramétrage de base est dans le répertoire gen/dyn/standard.
Il est possible de le modifier dans le répertoire custom ou dans un autre répertoire.
Le choix du paramétrage se fait à chaque génération.

=======================
le paramétrage standard
=======================

Le paramétrage standard est dans le répertoire gen/dyn/standard

========
Form.inc 
========
Voici les paramètres pour la génération de formulaire ::

    $serie = 15;                        nombre d'enregistrement par page'
    $ico ="../img/ico_application.png"; icone DEPRECATED ?
    $max=6;                             nb de ligne blob
    $taille=80;                         taille du blob
    $pgsql_taille_defaut = 20;          taille du champ par defaut si retour pg_field_prtlen =0
    $pgsql_taille_minimum    = 10;      taille minimum d affichage d un champ
    $pgsql_longueur_date=12;            taille d'affichage de la date ' 

========
Etat.inc 
========
Parametres ::

    $variable='&'; (indice precedent la variable)	
    // general
    $orientation='P';
    $format='A4';
    // footer
    $footerfont='helvetica';
    $footerattribut='I';
    $footertaille='8';
    // logo
    $logo='logopdf.png';
    $logoleft='58';
    $logotop='7';
    // titre
    $titreleft='41';
    $titretop='36';
    $titrelargeur='130';
    $titrehauteur='10';
    $titrefont='helvetica';
    $titreattribut='B';
    $titretaille='15';
    $titrebordure='0';
    $titrealign='C'; 
    // corps
    $corpsleft='7';
    $corpstop='57';
    $corpslargeur='195';
    $corpshauteur='5';
    $corpsfont='helvetica';
    $corpsattribut='';
    $corpstaille='10';
    $corpsbordure='0';
    $corpsalign='J';
    // sous etat
    $se_font='helvetica';
    $se_margeleft='8';
    $se_margetop='5';
    $se_margeright='5';
    $se_couleurtexte="array('0','0','0')";	

============
Sousetat.inc 
============

Parametres ::

    $longueurtableau= 195;
    $variable='&'; (indice precedent la variable)
    //titre
    $titrehauteur=10;
    $titrefont='helvetica';
    $titreattribut='B';
    $titretaille=10;
    $titrebordure=0;
    $titrealign='L';
    $titrefond=0;
    $titrefondcouleur="array(255,255,255)";
    $titretextecouleur="array(0,0,0)";
    // intervalle
    $intervalle_debut=0;
    $intervalle_fin=5;
    // entete
    $entete_flag=1;
    $entete_fond=1;
    $entete_orientation="array(0,0,0)";
    $entete_hauteur=7;
    $entete_fondcouleur="array(255,255,255)";
    $entete_textecouleur="array(0,0,0)";
    // tableau
    $tableau_bordure=1;
    $tableau_fontaille=10;
    // bordure
    $bordure_couleur="array(0,0,0)";
    // sous etat fond
    $se_fond1="array(243,246,246)";
    $se_fond2="array(255,255,255)";
    // cellule
    $cellule_fond=1;
    $cellule_hauteur=7;
    // total
    $cellule_fond_total=1;
    $cellule_fontaille_total=10;
    $cellule_hauteur_total=15;
    $cellule_fondcouleur_total="array(255,255,255)";
    // moyenne
    $cellule_fond_moyenne=1;
    $cellule_fontaille_moyenne=10;
    $cellule_hauteur_moyenne=5;
    $cellule_fondcouleur_moyenne="array(212,219,220)";
    // nombre d enregistrement
    $cellule_fond_nbr=1;
    $cellule_fontaille_nbr=10;
    $cellule_hauteur_nbr=7;
    $cellule_fondcouleur_nbr="array(255,255,255)";

=======
Pdf.inc 
=======

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


=========================
Customiser le paramétrage
=========================

Il est possible de personnaliser le paramétrage dans le répertoire custom ou en créant un autre répertoire avec des paramètres personnels.

Il faut mettre dans le répertoire le où les fichiers à personnaliser.

Ne personnaliser que les variables souhaitées dans le fichier. Par défaut, openMairie prendra les paramètres standards.

Choisir le paramétrage personnalisé dans l'écran de génération qui affiche les répertoires de paramètres existants.

Ne pas supprimer le répertoire  standard et les fichiers par défaut.