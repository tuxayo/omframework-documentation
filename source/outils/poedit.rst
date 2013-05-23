.. _poedit:

######
POEdit
######

POEdit est un éditeur de traductions de chaînes en plusieurs langues.
Les chaînes présentes dans l'interface des applications openMairie sont celles
présentes dans le code. Elles ne peuvent pas comporter d'accent.

Les étapes sont les suivantes :

1 - avoir dans le code .php les chaînes à traduire, en texte sans accent ni
    caractères spéciaux

2 - préparer les dossiers de traductions dans le dossier de locales

3 - avoir préalablement installé et configuré POEdit

4 - configurer le projet dans POEdit et effectuer scan du code afin de détecter
    les chaînes à traduire

5 - traduire les chaînes dans POEdit et sauvegarder

Spécification dans le code des chaînes à traduire
=================================================

Les chaînes peuvent être traduites, soit en français accentué, soit dans
d'autres langues. Pour cela il est nécessaires qu'elles soient présentes dans
les fichiers.php en respectant la syntaxe suivante ::

``_('Ma chaine a traduire, sans accent')``


Toutes les chaînes de caractères correspondant aux noms de tables et de champs
sont générées par le générateur et sont ainsi directement disponibles.

Préparation des dossiers de locales
===================================

Chaque application openMairie comporte, à la racine, un dossier appelé
``locales``.
Ce dossier comporte une structure de type ::

    locales/
        fr_FR/
            LC_MESSAGES/
                openmairie.po
                openmairie.mo

Il est nécessaire de créer un dossier de langue (par exemple ``en_US``) avec
son sous-dossier ``LC_MESSAGES`` pour chaque langue supplémentaire.

Le fichier .po contient les définitions de traductions ; il peut être modifié
au moyen de POEdit ou directement depuis un éditeur de texte simple.

Le fichier .mo contient les traductions sous une forme compilée. Il est généré
par POEdit automatiquement lors de chaque sauvegarde des traductions.

Installation et configuration de POEdit
=======================================

Installation
------------

POEdit est disponible nativement dans la plupart des systèmes linux. Il est
possible de le télécharger depuis le site officiel pour tous Linux, Windows et
Mac OSX.

http://www.poedit.net/download.php

Sous Linux Ubuntu ou Debian, il faut, en root, exécuter la commande
``apt-get install poedit``

Gestion de plusieurs langues
----------------------------

Une fois installé, il faut s'assurer que les ``locales`` (fichiers de définition
de langues) sont correctement installés sur le système.

Sous Linux Ubuntu, pour ajouter une locale, il faut ajouter sa définition dans
le fichier de pays correspondant (exemple : ``/var/lib/locales/supported.d/fr``)
puis lancer la commande, en root, ``dpkg-reconfigure locales``. Cela ne sera
nécessaire que pour ajouter la prise en charge d'une nouvelle langue.

Configuration d'un projet dans POEdit
=====================================

- Cliquer sur le bouton 'Créer un nouveau projet de traduction', même si le
  projet comporte déjà une traduction,

- Saisir le nom du projet et le chemin complet du dossier contenant le projet
  (chemin complet permettant d'aller à la racine du logiciel openMairie)

- Valider. Cela crée le projet. Les fichiers .po existants sont automatiquement
  détectés.

- Cliquer alors sur le bouton 'Mettre à jour tous les catalogues du projet'.
  Un scan de code est alors effectué par POEdit. Cela va parcourir tous les
  fichiers .php du dossier afin de détecter toutes les chaînes encadrées par
  ``_()``.
  
  Le scan peut comporter des erreurs (en général des accents dans les chaînes
  à traduire, des accents dans les commentaires du code, des chaînes à traduire
  vides etc.). Dans ce cas le fichier et la ligne concernée sont indiqués dans
  le rapport d'erreur. Il est recommandé de corriger l'erreur et recommencer
  la mise à jour.
  
  Le rapport affiche les chaînes désuètes (celles qui ne figurent plus dans le
  code) et les nouvelles chaînes. Les chaînes désuètes sont alors commentées
  dans le fichier .po et n'apparaissent plus dans l'interface de traduction.
  
Traduction des chaînes
======================

Depuis l'écran du projet dans POEdit, double-cliquer sur le fichier de la langue
concernée. La liste des chaînes à traduire apparaît alors.
Les nouvelles chaînes sont en premier, les chaînes modifiées en second et les
autres chaînes ensuite. Il suffit de cliquer sur une chaîne et entrer la
traduction dans le bloc de texte inférieur.

A chaque sauvegarde, le fichier est compilé en un fichier .mo.

L'affichage de l'application affiche alors les chaînes traduites à la place
des libellés originaux.

Attention, sur certaines configurations, un redémarrage du serveur web peut
être nécessaire pour que les traductions soient mises à jour.
