###########################
Stratégies de développement
###########################

On définit ici plusieurs stratégies de développement co-existant au sein de l'écosystème desapplications openMairie :

* SD01 - haut niveau de qualité 
* SD02 - niveau de qualité intermédiaire
* SD03 - niveau de qualité pauvre (Proof of Concept)

Ces stratégies ne se substituent pas aux règles de contribution (qui peut contribuer, et de quelle manière) en vigueur au sein de l'application concernée.
Elles sous-entendent également une adhésion complète au règles et aux bonnes pratiques de développement des applications openMairie présentes dans cette documentation.

====
SD01
====

Principes
_________

* Le tronc est toujours stable
* Chaque évolution / correction se fait dans une branche
* Chaque évolution / correction est documentée
* Technique de développement en TDD
* Tous les tests de l'application doivent passer

TDD (Test-Driven Development)
_____________________________

Le TDD, ou développement piloté par les tests repose sur le principe suivant ::

  Chaque implémentation d'une nouvelle fonctionnalité ou chaque correction de bug commence par l'écriture de tests.

Scénario-type de développement d'une évolution
______________________________________________

Initialisation
--------------

L'itération de développement commence par la création d'une branche dédiée sur le gestionnaire de contrôle de version (subversion, git).
Cette branche est issue du tronc, et doit être nommée explicitement selon la fonctionnalité implémentée.

Rédactions des tests
--------------------

Avant tout développement, des tests modélisant la fonctionnalité à implémenter sont rédigés. Selon le type d'évolution, il peut s'agir de :

* tests fonctionnels (on teste les scénarios utilisateur - Librairie RobotFramework)
* tests d'intégrations (comment s'intègre ma fonctionnalité dans les scénarios existannt - Librairie RobotFramework)
* tests unitaires (on teste méthodes et fonctions unitairement - Librairie PHPUnit)

Une fois rédigés, les tests sont lancés, et doivent être en échec (évolution non-implémentée).

Documentation
-------------

À ce stade, la nouvelle fonctionnalité doit être documentée dans le manuel utilisateur de l'application concernée.

Implémentation
--------------

Par la suite, l'implémentation commence, et se poursuit jusqu'à ce que les tests rédigés intialement passent.

Intégration
-----------

Tous les tests de l'application sont relancés et doivent passer avec succès.
Dans le cas où l'un d'entre-eux échoue, il doit être corrigé.

Incorporation
------------

L'évolution peut alors être incorporée sur le tronc de l'application. Pour ce faire, la branche est ré-actualisée avec les dernières modifications potentielles du tronc.
Dans le cas où le tronc a évolué depuis la création de la branche, l'étape d'intégration précedement décrite est conduite à nouveau.
La branche peut ensuite fusionnée dans le tronc.


Scénario-type de correction de bug
__________________________________

Le déroulement est similaire à celui décrit ci-dessus. Seule la méthodologie d'écriture des tests diffère :

* Dans le cas où il existe des tests couvrant la fonctionnalité dans laquelle est relevé le bug, ceux-ci sont altérés pour le prendre en compte.
* Dans le cas où il n'existe pas de scénario de tests permettant de matérialiser le bug, un nouveau test dédié est rédigé.
La correction du bug s'effectue alors, jusqu'à ce que les tests altérés ou nouvellement créés passent.

Bonnes pratiques
_______________

* Isolation des tests : chacun des tests ajouté doit être indépendant de ceux existant (consitution de son propre jeu de données, accès au ressources par recherche, etc)
* ...

====
SD02
====

====
SD03
====


