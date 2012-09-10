 .. _acces:

####################
La gestion des accès
####################

Le framework fournit un gestionnaire d'accès configurable dans :

- administration -> profil
- administration -> droit
- administration -> utilisateur

Les accès sont conservés dans les tables du même nom.

==========
Les tables
==========

La gestion des accès est gérée avec 3 tables :

**om_profil** : gestion par défaut de 5 profils :

- administrateur (5)
- super utilisateur (4)
- utilisateur (3)
- utilisateur limité (2)
- consultation (1)

Les profils sont hiérarchiques, le profil 5 étant le plus élevé, il a accès à
toutes les actions des profils inférieurs.

**om_droit**: la gestion des droits affecte un profil suivant chaque :

- action sur un objet métier : om_collectivite, om_parametre ...
- chaque action (du menu, d'un tableau, de consultation, ...)

Voir paramétrage menu : tableau Rubrik  right = "om_parametre"
            

**om_utilisateur** : cette table permet de donner un login, un mot de passe
et un profil à chaque utilisateur.
    
Diagramme de classe

.. image:: ../_static/acces_1.png

==========
Les règles
==========

Le droit sur un objet porte le nom de l'objet, pour chaque objet il existe deux
types de droits :

- généraux : il n'est composé que du nom de l'objet et permet d'accéder à toutes
  les actions sur celui-ci.
- specifique : il se compose du nom de l'objet puis d'un sufixe.

Détails des sufixes de droits :

- _tab : permet d'accéder au tableau
- _ajouter : permet d'ajouter un objet
- _modifier : permet de modifier l'objet
- _supprimer : permet de supprimer l'objet
- _consulter : permet de consulter l'objet

=====================
La multi-collectivité
=====================

Les collectivités peuvent être de niveau 1 ou de niveau 2. Les utilisateurs de
chaque collectivité heritent de ce niveau.
Les utilisateurs de niveau 1 n'ont accès qu'à leur collectivité tandis que les
utilisateurs de niveau 2 ont accès à toutes les collectivités disponibles.
Lors de la conception de la base de données un champ om_collectivite peut être
ajouté à chaque table ayant besoin d'un filtrage par collectivité.
Les utilisateurs de niveau 1 ne veront aucune notion de collectivité
et n'auront accès qu'aux éléments liés à la leur.


===================
Les login et logout
===================

Le login se fait par le script ``scr/login.php``

``login.php`` valorise les variables sessions permettant la gestion des accès
et securites


  .. code-block:: php

    <?php
      $_SESSION['profil'] = $profil;
      $_SESSION['nom'] = $nom;
      $_SESSION['login'] = $login;
    ?>

La déconnexion se fait avec le script  ``scr/logout``

Le changement de mot de passe se fait avec le script  ``scr/password.php``

L'accès au changement de mot de passe se fait par défaut dans le menu haut
(voir framework/paramétrage)


===============
Les utilitaires
===============

La gestion des droits d'accès se fait dans les méthodes des utilitaires :

- ``php/openmairie/om_application.class.php`` (composant openMairie)
- ``obj/utils.class.php``
    
(:ref:`voir framework/utilitaire<utilitaire>`)
