.. _edition:

############
Les éditions
############

Les éditions sont accessibles dans le menu par :

- administration -> etat

- administration -> sousetat

- administration -> lettretype



Depuis la version 4 d'openMairie, les editions sont conservées dans 3 tables :

- om_etat : pour les états

- om_sousetat : pour les sous etats

- om_lettretype : pour les lettres types



Cette modification a été faite pour pouvoir gérer la multi collectivité.

Par contre, les tableaux pdf sont stockés dans un fichier : nom_objet.pdf.inc 

================
Actif, non actif
================

Les sous etats sont liés a un ou plusieurs état

Les états, sous etats, et lettre type peuvent être actif ou non actif

Par défaut sont pris en compte :

1 - l'édition  "actif" de la collectivite

2 - l'édition "actif" de la multicollectivite

3 - l'édition "non actif" de la multicollectivite


Les editions non actifs d'une collectivite ne sont pas pris en compte


====================
Paramétrer des etats
====================

Il est conseillé d utiliser l'assistant état du generateur

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


Pour le corps, le titre et la requete sql, les zones entre crochets (exemple [nom]) sont les champs selectionnés par la requete.

Les variables commençant par "&" sont définies dans dyn/varpdf.inc (exemple &aujourdhui)
et dans la table om_parametre. 

=========================
Paramétrer des sous etats
=========================

Il est conseillé d utiliser l'assistant sousetat du générateur

Les paramétres  sont les suivants ::

    texte et caractéristique du Titre
    Intervalle avant et apres le tableau
    Entete de tableau (nom de colone)
    caracteristique du tableau
    caracteristique des cellules
    tatal, moyenne, nombre
    requete sql


Pour le titre et la requete sql, les zones entre crochets sont les champs selectionnés par la requete.

Les variables commençant par "&" sont définies dans dyn/varpdf.inc (exemple &aujourdhui)
et dans la table om_parametre 



===========================
Paramétrer des lettres type
===========================

Il est conseillé d utiliser l assistant lettretype du generateur

Les paramétres sont les suivants ::

    orientation portrait ou paysage
    format="A4", A3
    position et nom  du logo 
    titre de la lettre
    position et caractéristiques du titre
    corps de la lettre
    position et caractéristiques du corps
    la requete SQL


Pour le corps, le titre et la requete sql, les zones entre crochets  sont les champs selectionnés par la requete.

Les variables commençant par "&" sont définies dans dyn/varlettretypepdf.inc (exemple &aujourdhui)
et dans la table om_parametre 

==========================
Parametrer des edition pdf
==========================

Un etat pdf peut être généré par le generateur (option)

L'edition est paramétrée dans un fichier sql/sgbd/nom_objet.pdf.inc et dans la

Les paramétres sont les suivants ::

    texte et caractéristique du Titre
    Entete de tableau (nom de colone)
    caracteristique du tableau
    caracteristique des cellules
    tatal, moyenne, nombre
    requete sql

Pour le titre et la requete sql, les zones entre crochets sont les champs selectionnés par la requete.

Les variables commençant par "&" sont définies dans dyn/varpdf.inc (exemple &aujourdhui)
et dans la table om_parametre 

=========================
Parametrer les etiquettes
=========================

Les zones entre crochets  sont les champs selectionnés par la requete.
La variable  &aujourdhui sont définies dans dyn/varetiquettepdf.inc et dans la
table om_parametre

Il y aura une integration depuis l utilisation d'openPersonnalite dans la version openMairie 4.0.1..


=================
L'éditeur WYSIWYG
=================

Un editeur est prevu dans la prochaine version openMairie 4.0.1


===============
Les scripts PDF
===============



Les scripts sont dans le répertoire  **pdf/** et sont  appellés par le framework sous la forme ::

    pdfetat.php?obj=nom_etat&idx=enregistrement_a_editer

les scripts sont les suivants ::

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

Il n est pas prévu d integration dans la prochaine version 

==========
composants
==========

Les composants php sont stockés en **php/**

*/openmairie* 

Les scripts ci dessous sont les classes qui interfacent openmairie avec fpdf ::

    fpdf_etat.php
    fpdf_etiquette.php
    db_fpdf.php

*/fpdf*

    A ce niveau se situe le composant fpdf

*/phpmailer*

    la gestion de mail est EN TEST avec openPersonnalite et sera intégré dans  openMairie 4.0.1


Les composants javascript sont stockés dans le repertoire **lib/**

*/tinymce*

    est l'editeur wisiwig   EN TEST sur openRecensement et qui sera intégré dans openmairie 4.0.1)
