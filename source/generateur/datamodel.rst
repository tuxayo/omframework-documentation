=================================
Définition des modèles de données
=================================

L'identifiant
=============

Chaque modèle de données doit avoir un champ destiné à contenir l'identifiant
des objets. Sans ce champ, il n'est pas possible de créer un modèle.

Définition de l'identifiant
---------------------------

Il suffit d'ajouter la contrainte SQL ``PRIMARY KEY`` à une colonne d'une table
pour créer un champ identifiant. Il sera ensuite automatiquement géré par
openMairie lors de l'ajout, la modification et la suppression d'enregistrements.

Fonctionnement interne du générateur
------------------------------------

**Comment ce champ est déterminé lors de la génération d'un modèle?**

Le générateur suit la procédure suivante:

- il utilise la colonne ayant la contrainte ``PRIMARY KEY`` si elle existe;

- sinon, il utilise la colonne ayant le même nom que la table.

S'il n'existe ni contrainte, ni colonne ayant le même nom que la table, le
modèle ne peut pas être créé.

.. note::
   Le fait d'utiliser une colonne ayant le même nom que la table pour
   déterminer le champ identifiant est là pour une raison de
   rétro-compatibilité. Les versions d'openMairie antérieures à 4.3.0
   n'utilisaient pas encore la contrainte ``PRIMARY KEY``.

**Est-il possible d'utiliser une clé primaire composée de plusieurs colonnes?**

Non, et c'est bien mieux comme ça.

Les références vers d'autres objets
===================================

Un modèle de données peut contenir, un ou plusieurs champs, faisant référence
à d'autres objets. Ces objets pouvant être de modèle différent.

Définition des références
-------------------------

La méthode pour créer des références diffère en fonction du SGBD.

Avec PostgresSQL
................

Il suffit d'ajouter la contrainte SQL ``FOREIGN KEY`` à des colonnes pour créer
des champs de type référence.

Avec MySQL
..........

Il suffit de donner à une colonne le même nom que la table qu'elle référence.

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

Affichage dans les formulaires
------------------------------

**Comment ces champs sont représentés dans les formulaires?**

Depuis le formulaire de l'objet faisant référence, ces champs sont représentés
par des balises HTML ``<select>``. L'ensemble des objets pouvant être référencés
sont listés sous la forme d'options.

Depuis le formulaire de l'objet référencé, un onglet apparaît pour chaque
modèle différent faisant référence à cet objet. Chaque onglet liste l'ensemble
des objets faisant référence à l'objet présenté en formulaire.

Les champs uniques
==================

Un modèle de données peut contenir un ou plusieurs champs uniques. Il n'est
pas possible pour plusieurs objets d'un même modèle d'avoir la même valeur
pour ce champ.

Un modèle de données généré peut également contenir, au plus, un groupe de
champs unique. Cette fois, c'est la combinaison des valeurs de ces champs qui ne
pourra exister qu'une seule fois.

.. important::
   Actuellement **le générateur ne peut pas gérer plus d'un groupe de champs
   uniques** à la fois. S'il existe plusieurs groupes, ils devront être gérés
   manuellement par le développeur dans les fichiers de surcharge.

Définition des champs uniques
-----------------------------

Il suffit de définir une contrainte SQL ``UNIQUE`` sur une colonne ou un groupe
de colonnes pour créer respectivement un ou plusieurs champs uniques.

Fonctionnement interne du générateur
------------------------------------

**Comment ces champs sont déterminés lors de la génération d'un modèle?**

L'abstracteur de base de données d'openMairie peut, en analysant une table,
récupérer la liste de ses colonnes uniques.

Affichage dans les formulaires
------------------------------

**Comment ces champs sont représentés dans les formulaires?**

Ces champs sont affichés indifféremment des champs sans contrainte.

Lors de la validation d'un formulaire, une vérification est faite pour chaque
champ unique, ainsi que pour un éventuel groupe de champs uniques. Si une valeur
(ou combinaison) est déjà présente dans la base de données, un message d'erreur
est affiché, et la base de données n'est pas modifiée.

Les champs requis
=================

Un modèle de données peut contenir un ou plusieurs champs requis.

Définition des champs requis
----------------------------

Il suffit de définir une contrainte SQL ``NOT NULL`` sans clause ``DEFAULT`` sur
une colonne pour créer un champ requis.

.. attention::
   En ajoutant une clause ``DEFAULT`` a une contrainte ``NOT NULL`` nous
   indiquons clairement au générateur que **le champ n'est pas requis!** La
   valeur par défaut permet à l'utilisateur de laisser le champ vide lors d'une
   validation de formulaire. Le SGBD se charge alors d'ajouter lui même cette
   valeur.

Fonctionnement interne du générateur
------------------------------------

**Comment ces champs sont déterminés lors de la génération d'un modèle?**

L'abstracteur de base de données d'openMairie peut, en analysant une table,
récupérer la liste de ses colonnes requises n'ayant pas de valeur par défaut.

Affichage dans les formulaires
------------------------------

**Comment ces champs sont représentés dans les formulaires?**

Ces champs sont affichés avec un marqueur à côté de leur libellé, indiquant
qu'ils sont requis. Par défaut openMairie utilise le caractère ``*`` pour
indiquer les champs requis.

Si ces champs ne sont pas remplis lors de la validation d'un formulaire, un
message d'erreur est affiché pour chaque champ requis non complété, et la base
de données n'est pas modifiée.

Le champ libellé
================

Pour représenter des objets dans des champs de type ``<select>``, le générateur
utilise un champ textuel particulier appelé libellé.

Ce champ est également utilisé pour ordonner les éléments d'un tableau de
manière croissante.

Définition du libellé
---------------------

Pour définir explicitement une colonne comme libellé d'un modèle, il faut
la nommer ``libelle``.

Si cette colonne n'existe pas, le générateur considère la deuxième colonne
de la table comme étant un libellé (ce système était celui utilisé dans les
versions d'openMairie 4.2.0 et inférieures).

Enfin s'il n'existe pas de seconde colonne, la clé primaire de la table est
utilisé.
