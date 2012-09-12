====================
La recherche avancée
====================

Les différents types de recherche
=================================

Recherche simple
----------------

Cette recherche est celle disponible par défaut sur les tableaux d'openMairie.

Elle permet de:

- rechercher une valeur dans une colonne parmi toutes celles affichées;
- rechercher une valeur dans toutes les colonnes affichées;
- rechercher des valeurs approximatives.

.. note::
   Il est possible de modifier la liste des colonnes dans laquelle est effectuée
   la recherche. Cette liste n'est donc pas forcément celle affichéee. Elle
   l'est seulement par défaut, c'est à dire lorsqu'aucune surcharge ne modifie
   les fichiers générés dans ``gen/sql/``.

Recherche avancée
-----------------

Cette recherche est une fonctionnalité qui peut être activée et configurée
manuellement pour un ou plusieurs tableaux donnés.

Elle permet de:

- afficher un formulaire de recherche mono-critère permettant d'effectuer des
  recherches strictes ou approximatives;
- afficher un formulaire de recherche multi-critères permettant d'effectuer
  des recherches strictes ou approximatives;
- rechercher des valeurs dans des tables et des colonnes qui ne sont pas
  affichées.

Recherche avancée mono-critère
..............................

Le formulaire de recherche avancée mono-critère est un formulaire ne s'affichant
que si la recherche avancée est activée. Il permet aux utilisateurs de basculer
sur un formulaire similaire à celui de recherche simple lorsque la recherche
avancée est activée.

Ce formulaire se comporte de la même manière que celui de recherche simple, avec
quelques différences:

- il permet de rechercher des valeurs strictes ou approximatives;
- il conserve les valeurs recherchées après la réalisation d'une action (ajout,
  modification, etc...);
- il dispose d'un bouton ``Vider le formulaire`` permettant de vider les champs;
- il dispose d'un bouton ``+`` permettant de basculer sur le formulaire
  multi-critères.

Recherche avancée multi-critères
................................

Le formulaire de recherche avancée multi-critères est un formulaire ne
s'affichant que si la recherche avancée est activée. Il permet aux utilisateurs
de bénéficier de plusieurs champs, et ainsi effectuer des recherches plus
précise qu'avec le formulaire de recherche simple.

Description du formulaire:

- il peut afficher plusieurs champs, de type texte, nombre, date ou liste à
  choix;
- il permet, pour chaque tableau, de configurer la liste des champs affichés;
- il permet, pour chaque champ, de rechercher des valeurs strictes ou
  approximatives;
- il permet, pour chaque champ, de rechercher des valeurs dans des tables et
  des colonnes qui ne sont pas affichées;
- il conserve les valeurs recherchées après la réalisation d'une action (ajout,
  modification, etc...);
- il dispose d'un bouton ``Vider le formulaire`` permettant de vider les champs;
- il dispose d'un bouton ``+`` permettant de basculer sur le formulaire
  mono-critère.

Activer et configurer la recherche avancée
==========================================

Configuration des critères de recherche
=======================================
