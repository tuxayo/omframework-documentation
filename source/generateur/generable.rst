========================
Conditions de génération
========================

Contraintes de la base de données
=================================

Pour qu'un modèle de données puisse être généré, il faut que la table de base
de donneés qui le représente remplisse les conditions suivantes:

- la table doit avoir une clé primaire composée d'une seule colonne, ou une
  colonne portant le même nom que la table;

- les clés étrangères doivent référencer des tables remplissant la condition
  ci-dessus.

Si l'une de ces conditions n'est pas satisfaite, les interfaces de génération
affichent une erreur.

Contraintes du système de fichiers
==================================

Le générateur crée les classes métier de l'application ainsi que les fichiers
de surcharge. Pour pouvoir créer ces fichiers, le serveur web PHP doit avoir les
droits d'écriture dans les dossiers suivants:

- gen/obj
- gen/sql/pgsql
- gen/sql/mysql
- obj/
- sql/pgsql
- sql/mysql

Si des droits sont manquants:

- les scripts ``genauto`` et ``gensup`` ne permettent pas de générer ces
  fichiers que le serveur ne peut pas écrire;
- le script ``genfull`` quant-à lui, affiche des messages après la génération
  indiquants quels fichiers ne sont pas accessibles (ces fichiers ne sont, bien
  entendu, pas générés).

