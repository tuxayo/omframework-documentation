========================
Conditions de génération
========================

Contraintes de la base de données
=================================

Pour qu'un modèle de données puisse être généré, il faut que la table de base
de donneés qui le représente remplisse les conditions suivantes:

- La table doit avoir une clé primaire composée d'une seule colonne, ou une
  colonne portant le même nom que la table.

- Les clés étrangères doivent référencer des tables remplissant la condition
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
