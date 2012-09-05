=================================
Définition des modèles de données
=================================

Précision sur le vocabulaire utilisé sur cette page.

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

L'identifiant
=============

Chaque modèle de données doit avoir un champ destiné à contenir l'identifiant
des objets. Sans ce champ, il n'est pas possible de créer le modèle.

Définition de l'identifiant
---------------------------

Il suffit de définir une colonne de la table représentant le modèle comme clé
primaire opur créer un champ identifiant. Il sera ensuite automatiquement géré
par openMairie lors de l'ajout, la modification et la suppression
d'enregistrements.

Fonctionnement interne du générateur
------------------------------------

**Comment ce champ est déterminé lors de la génération d'un modèle?**

Le générateur suit la procédure suivante:

- il utilise la clé primaire de la table si elle existe;

- sinon, il utilise la colonne ayant le même nom que la table.

Si il n'existe ni de clé primaire, ni de colonne ayant le même nom que la table,
le modèle ne peut pas être créé.

.. note::
   Le fait d'utiliser une colonne ayant le même nom que la table pour
   déterminer le champ identifiant est là pour une raison de
   rétro-compatibilité. Les versions d'openMairie antérieures à 4.3.0
   n'utilisaient pas encore les clés primaires.

**Est-il possible d'utiliser une clé primaire composée de plusieurs colonnes?**

Non, et c'est bien mieux comme ça.

Les références vers d'autres objets
===================================

Un modèle de données peut contenir, un ou plusieurs champs, faisant référence
à d'autres objets. Ces objets pouvant être de modèle différent.

Définition des références
-------------------------

.. attention::
   La définition des références ne se fait pas de la même manière en fonction
   du SGBD.

Avec PostgresSQL
................

Dans une table donnnée, les références sont représentées par des clés
étrangères et des colonnes portant le même nom que d'autres tables.

Avec MySQL
..........

Dans une table donnnée, les références sont représentées par des colonnes
portant le même nom que d'autres tables.

Fonctionnement interne du générateur
------------------------------------

**Comment ces champs sont déterminés lors de la génération d'un modèle?**

Le générateur conserve une liste des colonnes qui donneront, après génération,
des champs références. Pour créer cette liste, il suit la procédure suivante:

- avec PostgresSQL, le générateur peut interroger les tables du système
  contenant la liste des clés étrangères d'une table particulière, ainsi que les
  tables étrangères référencées par ces clés;

- dans un second temps, indifféremment du SGBD, il ajoute a la liste des
  clés étrangères le nom des colonnes portant le même nom que d'autres tables.

La liste ainsi formée permettra au générateur de créer des champs de type
référence dans les modèles de données.

**Comment ces champs sont représentés dans les formulaires?**

Depuis le formulaire de l'objet faisant référence, ces champs sont représentés
par des balise HTML ``<select>``. L'ensemble des objets pouvant être référencés
sont listés sous la forme d'option.

Depuis le formulaire de l'objet référencé, un onglet apparaît pour chaque
modèle différent faisant référence à cet objet. Chaque onglet liste l'ensemble
des objets faisant référence à l'objet présenté en formulaire.

Les champs uniques
==================

Un modèle de données peut contenir un ou plusieurs champs uniques. Il n'est
pas possible pour plusieurs objets d'un même modèle d'avoir la même valeur
pour ce champ.

Un modèle de données peut également contenir, au plus, un groupe de champs
unique. Cette fois, c'est la combinaison des valeurs de ces champs qui ne
pourra exister qu'une seule fois.

Définition de champ unique
--------------------------

Il suffit de définir une contrainte SQL ``UNIQUE`` sur une colonne ou un groupe
de colonne pour créer respectivement un ou plusieurs champs uniques.

Fonctionnement interne du générateur
------------------------------------

Les champs requis
=================

Définition des références
-------------------------

Fonctionnement interne du générateur
------------------------------------
