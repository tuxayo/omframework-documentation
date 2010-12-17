.. _edition:

#######
edition
#######

Les éditions sont accessibles dans le menu par :

- administration -> etat

- administration -> sousetat

- administration -> lettretype


Depuis la version 4 d'openMairie, les editions sont conservées dans 3 tables :

- om_etat : pour les états

- om_sousetat : pour les sous etats

- om_lettretype : pour les lettres types

Cette modification a été faite pour pouvoir gérer la multi collectivité.

Par contre, les tableaux pdf sont stockés dans un fichier : nom_table.pdf.inc 

==============
fonctionnement
==============

Les sous etats sont liés a un ou plusieurs sous état

Etats, sous etats, et lettre type peuvent être :

* actif / non actif

Par défaut sont pris en compte :

1 l edition  actif de la collectivite

2 l'edition actif de la multicollectivite

3 l edition non actif de la multicollectivite

Les editions non actifs d'une collectivite ne sont pas pris en compte

=====================
Paramétrage des etats
=====================

Il est conseille d utiliser l assistant etat du generateur

Les paramètres sont les suivants ::

    orientation portrait ou paysage
    format="A4", A3
    position et nom  du logo 
    titre de l etat
    position et caractéristiques du titre
    corps de l etat
    position et caractéristiques du corps
    la requete SQL
    les sous etats associés et les caractéristiques


Pour le corps, le titre et la requete sql :

les zones entre crochets (exemple [nom]) sont les champs selectionnés par la requete.

La variable &ville $datecourrier sont définies dans dyn/varpdf.inc

==========================
Paramétrage des sous etats
==========================

Il est conseille d utiliser l assistant etat du generateur

Les paramétres  sont les suivants ::

    texte et caractéristique du Titre
    Intervalle avant et apres le tableau
    Entete de tableau (nom de colone)
    caracteristique du tableau
    caracteristique des cellules
    tatal, moyenne, nombre
    requete sql


Pour le titre et la requete sql :

les zones entre crochets sont les champs selectionnés par la requete.

La variable &ville $datecourrier sont définies dans dyn/varpdf.inc



============================
Paramétrage des lettres type
============================

Il est conseille d utiliser l assistant etat du generateur

Les paramètres sont les suivants ::

    orientation portrait ou paysage
    format="A4", A3
    position et nom  du logo 
    titre de la lettre
    position et caractéristiques du titre
    corps de la lettre
    position et caractéristiques du corps
    la requete SQL


Pour le corps, le titre et la requete sql :

les zones entre crochets  sont les champs selectionnés par la requete.

La variable  &aujourdhui sont définies dans dyn/varlettretypepdf.inc et dans la
table om_parametre

===========================
parametrage des edition pdf
===========================

Un etat pdf peut être genere par le generateur (option)

L'edition est paramétrée dans un fichier sql/sgbd/nom_table.pdf.inc et dans la

Les paramétres sont les suivants ::

    texte et caractéristique du Titre
    Entete de tableau (nom de colone)
    caracteristique du tableau
    caracteristique des cellules
    tatal, moyenne, nombre
    requete sql

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

Un editeur est prevu dans la prochaine version openMairie 4.0.1


=======
scripts
=======

**pdf/** ::

    pdfetat.php : etat et sous etat
    pdf.php : edition pdf
    pdfetiquette.php : etiquette
    pdflettretype.php

pdfEtiquette sera repris dans la version 4.0.1 d'openMairie

**specifique openCourrier pour ecriture sur pdf** ::

    fpdf_tpl.php
    fpdi.php
    fpdi2tcpdf_bridge.php
    fpdi_pdf_parser.php
    histo.htm
    pdf_context.php
    pdf_parser.php
    testfpdi.php

il n est pas prévu d integration dans la prochaine version

==========
composants
==========

**php/**

*/openmairie* ::

    fpdf_etat.php
    fpdf_etiquette.php
    db_fpdf.php

*/fpdf*

A ce niveau se situe le composant fpdf

*/phpmailer*

gestion de mail (openPersonnalite) EN TEST pour openMairie 4.0.1


**lib/**

*/tinymce* : editeur wisiwig (test sur openrecencement  EN TESTopenmairie 4.0.1)
