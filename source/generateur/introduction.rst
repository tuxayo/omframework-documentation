.. _gen-introduction:

============
Introduction
============

**Le générateur permet de construire des applications à partir de l'analyse d'un
schéma d'une base de données.**

Les informations récupérées dans le schéma sont les suivantes:

- la liste des tables de la base de données;
- le nom, le type et les contraintes de chaque colonne.

Sur cette analyse le générateur crée les modèles de données. openMairie gère:

- les clés primaire mono-colonne;
- les clés étrangères;
- la multi-collectivité (selon la présence d'un champ ``om_collectivite``).

**Le générateur contient également des assistants permettant de créer
facilement des états, associés des collectivité.**
