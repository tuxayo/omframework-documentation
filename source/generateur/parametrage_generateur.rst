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
    $ico ="../img/ico_application.png"; icone DEPRECATED 
    $max=6;                             nb de ligne blob
    $taille=80;                         taille du blob
    $pgsql_taille_defaut = 20;          taille du champ par defaut si retour pg_field_prtlen =0
    $pgsql_taille_minimum    = 10;      taille minimum d affichage d un champ
    $pgsql_longueur_date=12;            taille d'affichage de la date ' 


EN cours de reflexion ::

    prendre en compte la valeur réélle de la longueur des
    champs en postgresql
    paramétrer un système d'exclusion de champ (exemple geom)
    exclusion de cle secondaire (exemple parcelle ou pos dans dossier d openfoncier)


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