.. _edition:

############
Les éditions
############

openMairie permet d'effectuer des éditions au format PDF. Ces éditions
sont paramétrées depuis l'interface du logiciel. Pour chaque édition PDF des
champs de fusion permettent de récupérer dynamiquement les données à imprimer.

Les éditions sont accessibles dans le menu par ::

    - parametrage -> etat 
    - parametrage -> sousetat
    - parametrage -> lettretype

Depuis la version 4 d'openMairie, les éditions sont conservées dans 3 tables ::

    - om_etat : pour les états  
    - om_sousetat : pour les sous-états
    - om_lettretype : pour les lettres types

Contrairement aux états et aux lettres-types, les tableaux de bord PDF sont
stockés dans un fichier : nom_objet.pdf.inc .

================
Actif, non actif
================

Les sous-etats sont liés a un ou plusieurs état.

Les états, sous-états, et lettre type peuvent être "actif" ou "non-actif".

Par défaut sont pris en compte :

1 - l'édition  "actif" de la collectivité

2 - l'édition "actif" de la multicollectivité

3 - l'édition "non-actif" de la multicollectivité


Les éditions d'une collectivité ayant le statut "non-actif" ne sont pas prises
en compte.


====================
Paramétrer des états
====================

Il est conseillé d'utiliser l'assistant état du générateur.

Les paramètres sont les suivants ::

    orientation portrait ou paysage
    format="A4", A3
    position et nom  du logo 
    titre de l état
    position et caractéristiques du titre
    corps de l état
    position et caractéristiques du corps
    la requête SQL
    les sous-états associés et les caractéristiques


Pour le corps et le titre, les zones entre crochets (exemple [nom]) sont les
champs de fusion, sélectionnés par la requête SQL. 

Ces champs de fusion peuvent être mis en majucule ou en minuscule selon les 
besoins grâce aux balises <MAJ></MAJ> et  <min></min>.

EX. :

Requête SQL = SELECT nom as nom FROM A;

Cette phrase dans un PDF 
'Lorem [nom] dolor sit amet, <MAJ>[nom]</MAJ> adipiscing elit. Curabitur feugiat.'
deviendra
'Lorem nom dolor sit amet, NOM adipiscing elit. Curabitur feugiat.'

Les variables commençant par "&" sont celles définies dans dyn/varpdf.inc
(exemple &aujourdhui) et dans la table om_parametre.

=========================
Paramétrer des sous-états
=========================

Il est conseillé d'utiliser l'assistant sous-etat du générateur.

Les paramètres  sont les suivants ::

    texte et caractéristique du Titre
    Intervalle avant et après le tableau
    Entête de tableau (nom de colonne)
    caractéristique du tableau
    caractéristique des cellules
    total, moyenne, nombre
    requête SQL


Pour le titre, les zones entre crochets sont les champs de fusion,
sélectionnés par la requête.

Les variables commençant par "&" sont celles définies dans dyn/varpdf.inc
(exemple &aujourdhui) et dans la table om_parametre.

===========================
Paramétrer des lettres-type
===========================

Il est conseillé d'utiliser l'assistant lettre-type du générateur.

Les paramètres sont les suivants ::

    orientation portrait ou paysage
    format="A4", A3
    position et nom  du logo 
    titre de la lettre
    position et caractéristiques du titre
    corps de la lettre
    position et caractéristiques du corps
    la requête SQL


Pour le corps et le titre, les zones entre crochets  sont les champs de fusion,
sélectionnés par la requête.

Les variables commençant par "&" sont celles définies dans
dyn/varlettretypepdf.inc (exemple &aujourdhui) et dans la table om_parametre.

==========================
Paramétrer des édition PDF
==========================

Un état PDF peut être généré par le générateur (option).

L’édition est paramétrée dans un fichier sql/sgbd/nom_objet.pdf.inc et dans la

Les paramètres sont les suivants ::

    texte et caractéristique du Titre
    Entête de tableau (nom de colonne)
    caractéristique du tableau
    caractéristique des cellules
    total, moyenne, nombre
    requête SQL

Pour le titre, les zones entre crochets sont les champs de fusion, sélectionnés
par la requête.

Les variables commençant par "&" sont celles définies dans dyn/varpdf.inc
(exemple &aujourdhui) et dans la table om_parametre.

=========================
Paramétrer les étiquettes
=========================

Les zones entre crochets  sont les champs de fusion sélectionnés par la requête.
Les variables (exemple &aujourdhui) sont celles définies dans
dyn/varetiquettepdf.inc et dans la table om_parametre.

Il y aura une integration depuis l'utilisation d'openPersonnalite dans une
prochaine version openMairie.

=================
L'éditeur WYSIWYG
=================

Un éditeur avancé a été mis en place dans cette version openMairie afin de
permettre à l'utilisateur de définir des mises en forme complexes. (tinymce)

===============
Les scripts PDF
===============

Les scripts sont dans le répertoire  **pdf/** et sont  appelés par le framework
sous la forme ::

    pdfetat.php?obj=nom_etat&idx=enregistrement_a_editer

les scripts sont les suivants ::

    pdfetat.php : état et sous-état
    pdf.php : édition PDF
    pdfetiquette.php : étiquette
    pdflettretype.php

pdfEtiquette sera repris dans une prochaine version d'openMairie

**specifique openCourrier pour ecriture sur pdf** ::

    fpdf_tpl.php
    fpdi.php
    fpdi2tcpdf_bridge.php
    fpdi_pdf_parser.php
    histo.htm
    pdf_context.php
    pdf_parser.php
    testfpdi.php

Il n'est pas prévu d'intégration dans la prochaine version.

==========
Composants
==========

*/core* 

Les scripts ci dessous sont les classes qui interfacent openmairie avec fpdf ::

    fpdf_etat.php
    fpdf_etiquette.php
    db_fpdf.php

*php/fpdf*

    A ce niveau se situe le composant fpdf
