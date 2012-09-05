.. _gen-introduction:

============
Introduction
============

**Le générateur permet de construire des applications à partir de l'analyse d'un
schéma d'une base de données.**

Les informations récupérées dans le schéma de la base de données sont les
suivantes:

- la liste des tables de la base de données;
- le nom, le type et la longueur de chaque colonne.

Sur cette analyse, le générateur construit les modèles de données en prennant
en compte:

- la clé primaire;

- les clés étrangères;

- la multi-collectivité si un champ ``om_collectivite`` existe.

**Le générateur contient des assistants permettant de créer facilement des
états, associés à une collectivité.**
