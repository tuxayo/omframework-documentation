.. _parametrage:

#######
edition
#######

====
menu
====

administration -> etat
administration -> sousetat
administration -> lettretype

=====
table
=====

Depuis la version 4 d'openMairie, les editions sont conservées dans 3 tables :

- om_etat : pour les états
- om_sousetat : pour les sous etats
- om_lettretype : pour les lettres types

Cette modification a été faite pour pouvoir gérer la multi collectivité

==============
fonctionnement
==============

Les sous etats sont liés a un ou plusieurs sous état


* actif / non actif
Par défaut sont pris en compte :
1 l edition  actif de la collectivite
2 l'edition actif de la multicollectivite
3 l edition non actif de la multicollectivite

Les editions non actifs d'une collectivite ne sont pas pris en compte

=====================
Parematrage des etats
=====================

Il est conseille d utiliser l assistant etat du generateur

- orientation portrait ou paysage
- format="A4", A3
- position et nom  du logo 
- titre de l etat
- position et caractéristiques du titre
- corps de l etat
- position et caractéristiques du corps
- la requete SQL
- les sous etats associés et les caractéristiques

Pour le corps, le titre et la requete sql :
les zones entre crochets (exemple [nom]) sont les champs selectionnés par la requete.
La variable &ville $datecourrier sont définies dans dyn/varpdf.inc

==========================
Parematrage des sous etats
==========================

Il est conseille d utiliser l assistant etat du generateur


- texte et caractéristique du Titre
- Intervalle avant et apres le tableau
- Entete de tableau (nom de colone)
- caracteristique du tableau
- caracteristique des cellules
- tatal, moyenne, nombre
- requete sql

Pour le titre et la requete sql :
les zones entre crochets sont les champs selectionnés par la requete.
La variable &ville $datecourrier sont définies dans dyn/varpdf.inc



============================
Parematrage des lettres type
============================

Il est conseille d utiliser l assistant etat du generateur

- orientation portrait ou paysage
- format="A4", A3
- position et nom  du logo 
- titre de la lettre
- position et caractéristiques du titre
- corps de la lettre
- position et caractéristiques du corps
- la requete SQL

Pour le corps, le titre et la requete sql :
les zones entre crochets  sont les champs selectionnés par la requete.
La variable  &aujourdhui sont définies dans dyn/varlettretypepdf.inc et dans la
table om_parametre

===========================
parametrage des edition pdf
===========================

Un etat pdf peut être genere par le generateur (option)
L'edition est paramétrée dans un fichier sql/sgbd/nom_table.pdf.inc et dans la
table om_parametre

- texte et caractéristique du Titre
- Entete de tableau (nom de colone)
- caracteristique du tableau
- caracteristique des cellules
- tatal, moyenne, nombre
- requete sql

Pour le titre et la requete sql :
les zones entre crochets sont les champs selectionnés par la requete.
La variable  &aujourdhui sont définies dans dyn/varpdf.inc et dans la
table om_parametre


==========================
parametrage des etiquettes
==========================

openMairie 4.0.1

les zones entre crochets  sont les champs selectionnés par la requete.
La variable  &aujourdhui sont définies dans dyn/varetiquettepdf.inc et dans la
table om_parametre

Il y aura une integration depuis l utilisation d'openPersonnalite


===============
Editeur WYSIWYG
===============

openMairie 4.0.1


=======
scripts
=======

pdf/

pdfetat.php : etat et sous etat
pdf.php : edition pdf
pdfetiquette.php : etiquette
pdflettretype.php

specifique openCourrier pour ecriture sur pdf

fpdf_tpl.php
fpdi.php
fpdi2tcpdf_bridge.php
fpdi_pdf_parser.php
histo.htm
pdf_context.php
pdf_parser.php
testfpdi.php

il n est pas prévu d integration dans le framework

==========
composants
==========

php/

/openmairie
fpdf_etat.php
fpdf_etiquette.php
db_fpdf.php

/fpdf

EN TEST
/phpmailer
gestion de mail (openPersonnalite)
openMairie 4.0.1


lib/

EN TEST
/tinymce : editeur wisiwig (test sur openrecencement openmairie 4.0.1)
