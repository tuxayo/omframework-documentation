.. _analyse_base:

####################
l analyse de la base
####################

Les informations de la base sont analysées par la méthode « constructeur » de gen.class.php 

La construction des formulaires se fait suivant 4 types de champs reconnus par le générateur:
- string : chaîne de caractère 
- int : nombre (entier ou décimal)
- date 
- blob : texte

==============
Type de champs
==============

la champ String est du type openMairie (méthode setType()):

- text dans le cas général

- hiddenstatic si modification pour clé primaire

- select pour clé secondaire

Le champ date est du type openMairie date avec calendrier et java script de contrôle de saisie de date

(La date est au format français JJ/MM/AAAA)

le champ Int est du type openMairie (methode setType())

- hidden si clé primaire en ajout

- hiddenstatic si clé primaire en modification

- text avec contrôle numérique en javascript

- select pour clé secondaire

Le champ Blob est du type openMairie textarea

La longueur et la largeur sont définis en fichier de paramétrage form.inc

La taille n est pas pris en compte dans la longueur d'enregistrement

Les paramètres de dyn/form.inc permettent d'établir la longueur et la largeur d'affichage d'un blob :
	$max=6; // nombre de ligne blob
	$taille=80; // taille du blob


========================================
Equivalence type mysql / type openMairie
========================================

type mysql (longueur)          tableinfo   -> type openMairie ::

    Int         (taille mysql)                  -> Int
    Date        (10)                            -> date 
    Blob        (65535)                         -> Blob
    Char        (taille mysql)      char        -> String
    tinyint     (4)                 tinyint     -> Int
    smallint    (6)                 smallint    -> Int
    Mediumint   (9)                 mediumint   -> Int
    Bigint      (20)                bigint      ->Int
    Float       (12)                Real        -> Int
    Double      (22)                Real        -> Int
    Decimal     (11)                Real        -> Int
    Text        (65535)             Blob        -> Blob
    Tinyblob    (255)               blob        ->Blob
    Mediumblob  16777615            Blob        -> Blob
    Mediumtext  16777215            Blob        -> Blob
    Longtext    -1                  blob        -> Blob
    Longblob    -1                  Blob        -> Blob
    Tinytext    255                 Blob -      -> blob



========================================
Equivalence type pgsql / type openMairie
========================================

L'information fournie par postgresql est moins complète que celle de mysql surtout au niveau de la longueur des champs « string » où il est fourni :la longueur de stockage  qui est égal à -1 quand le stockage est variable


type pgsql (longueur) type tableinfo si different -> type openMairie ::

    Bigint      (8)                 int8        -> int
    Smallint    (2)                 Int2        -> Int
    Integer     (4)                 Int4        -> Int
    Real        (4)                 Float4      -> Int
    Doubleprecision (8)             Float8      -> Int
    Numeric     (20)                Numeric     -> Int
    Money       (8)                 Money       -> Int
    Char        (1)                 Char        -> String   (Quelque soit la longueur= 1)
    Character   (-1)                Bpchar      -> String (Utilisation de la longueur d'affichage)
    Character varying (-1)          Varchar     -> String (Utilisation de la longueur d'affichage)
    Text        (-1)                text        -> blob  (Utilisation des paramètres de form.inc)
    Date        (4)                 Date        -> Date (Utilisation des paramètres de form.inc - $pgsql_longueur_date)



Gestion des longueurs négatives :


Il est remonté dans l'analyse des informations de la base une valeur négative car le stockage dans postgresql est variable. 
OpenMairie utilise alors la longueur d'affichage qui est donnée par la plus longue saisie dans un champ. Donc pour avoir la longueur exacte, il est conseillé de saisie un enregistrement avec les champs « string » saisie avec le nombre de caractères souhaités à l'affichage.

Dans le cas ou rien n'est saisi, il est proposé dans form.inc ::

    $pgsql_taille_defaut = 20; // taille du champ par défaut si retour pg_field_prtlen =0
    $pgsql_taille_minimum = 10; // taille minimum d affichage d un champ


=====================
Nom de champ et table
=====================

Attention au nom de tables ou de champs, évitez les termes SQL : match, table, index, type, len ... ou openMairie : objet pour les noms de champs ou table

Specifique au generateur :

cle primaire de la table = nom de la table

cle secondaire = nom de la table fille
