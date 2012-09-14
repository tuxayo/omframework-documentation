========================
Fonctionnalités avancées
========================

Ajouter une date de validité à un modèle
========================================

Description
-----------

Le générateur permet de créer des objets qui seront considérés comme valides
seulement pendant une période donnée.

**Qu'est ce qu'un objet à date de validité?**

Un objet à date de validité est un objet contenant deux champs spécifiques:

- ``om_validite_debut``, déterminant la date de début de validité;
- ``om_validite_fin``, déterminant la date de fin de validité.

Un objet valide se comporte comme un objet « traditionnel ». Par contre,
lorsqu'il arrive à expiration, l'objet n'apparaît plus dans les tableaux, les
sous-tableaux et les champs de sélection (sauf s'il est actuellement valeur de
l'un de ses champs).

Un tel objet est valide lorsque:

- sa date de début de validité est nulle ET (sa date de fin de validité est
  nulle OU sa date de fin de validité est strictement supérieure à la date
  actuelle) OU,
- sa date de début de validité est inférieure ou égale à la date actuelle ET (sa
  date de fin de validité est nulle OU sa date de fin de validité est
  strictement supérieure à la date actuelle).

A l'inverse, il est considéré comme non-valide (expiré) lorsque:

- il n'est pas valide.

**Est-il possible de consulter la liste des objets expirés d'un modèle donné?**

Oui.

Le tableau du modèle dispose d'un bouton ``Afficher les éléments expirés``
permettant d'afficher, en plus des objets valides, les objets expirés. Lorsque
les éléments expirés sont affichés, bouton devient
``Masquer les éléments expirés``.

Définition des dates de validité
--------------------------------

Pour que le générateur considère un modèle comme « à date de validité », il
faut que sa table de base de données contienne les deux colonnes suivantes:

- ``om_validite_debut DATE``;
- ``om_validite_fin DATE``.

.. important::
   Il ne faut surtout pas définir de contrainte ``NULL`` ou ``DEFAULT`` sur ces
   deux colonnes, sinon ces champs seront obligatoires à chaque validation de
   formulaire.

Affichage dans les formulaires
------------------------------

Ces champs apparaissent dans les formulaires sous la forme de ``datepicker``,
comme des champs de type date classiques.
