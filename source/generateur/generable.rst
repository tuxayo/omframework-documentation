========================
Conditions de génération
========================

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

Contraintes de la base de données
=================================

Pour qu'un modèle de données puisse être généré, il faut que la table de base
de donneés qui le représente remplisse les conditions suivantes:

- la table doit avoir une clé primaire composée d'une seule colonne, ou une
  colonne portant le même nom que la table;

- les clés étrangères doivent référencer des tables remplissant la condition
  ci-dessus.

Si l'une de ces conditions n'est pas satisfaite, l'interface génération affiche
une erreur. Cette interface sera vu en détail plus loin.

Contraintes du système de fichiers
==================================

Le générateur cré les classes métier de l'application ainsi que les fichiers
de surcharge. Pour pouvoir créer ces fichiers, le serveur web PHP doit avoir les
droits d'écriture dans les dossiers suivants:

- gen/obj
- gen/sql/pgsql
- gen/sql/mysql
- obj/
- sql/pgsql
- sql/mysql

Si des droits sont manquant des messages erreurs seront affichés après la
génération indiquant quel fichier n'est pas accessible.
