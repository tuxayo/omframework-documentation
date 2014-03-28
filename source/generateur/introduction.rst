.. _gen-introduction:

Précision sur le vocabulaire utilisé dans cette documentation.

**Modèle de données**

    *« En informatique, un modèle de données est un modèle qui décrit de façon
    abstraite comment sont représentées les données dans une organisation
    métier, un système d'information ou une base de données. »*

\- cf. Wikipédia, Article `Modèle de données <http://fr.wikipedia.org/wiki/Mod%C3%A8le_de_donn%C3%A9es>`_.

Dans la documentation suivante, le terme modèle de données est utilisé pour
désigner les classes métier d'openMairie ainsi que les formulaires qu'elles
représentent.

**Objet**

Le mot objet fait référence aux instances des classes d'openMairie et, par
extension, aux enregistrements en base de données qui les représentent.

.. note ::  la génération avec mysql est abandonnée avec cette version qui ne conserve que postgresql


============
Introduction
============

**Le générateur permet de construire des applications à partir de l'analyse d'un
schéma d'une base de données.**

Les informations récupérées dans le schéma sont les suivantes:

- la liste des tables;
- le nom, le type et les contraintes de chaque colonne.

Sur cette analyse le générateur crée les modèles de données. openMairie gère:

- les clés primaires mono-colonne;
- les clés étrangères;
- la multi-collectivité (selon la présence d'un champ ``om_collectivite``).

**Le générateur contient également des assistants permettant de créer
facilement des états associés à des collectivités.**
