 .. _acces:

####################
La gestion des accès
####################

Le framework fournit un gestionnaire d'accés configurable dans :

    - administration -> profil
    - administration -> droit
    - administration -> utilisateur

Les accès sont conservés dans les tables du même nom.

==========
Les tables
==========

La gestion des accès est gérée avec 3 tables :

**om_profil** : gestion par defaut de 5 profils :

    - administrateur
    - super utilisateur
    - utilisateur
    - utilisateur limite
    - consultation

**om_droit**: la gestion des droits affecte un profil suivant chaque :

    action sur un objet métier : $obj om_collectivite, om_parametre ...

    chaque rubrique du menu :

    voir paramétrage menu : tableau Rubrik  right = "om_parametre"
            

**om_utilisateur** : cette table permet de donner un login, un mot de passe
et un profil à chaque utilisateur

    
    
Diagramme de classe

.. image:: ../_static/acces_1.png

==========
Les règles
==========

- le droit sur un objet porte le nom de l'objet, pour chaque objet il existe deux types de droits :

    - généraux : il n'est composé que du nom de l'objet et permet d'accéder à toutes les action sur celui-ci.
    - limité : il se compose du nom de l'objet puis d'un sufixe.

- Détails des sufixes de droits :

    - _tab : permet d'accéder au tableau
    - _ajouter : permet d'ajouter un objet
    - _modifier : permet de modifier l'objet
    - _supprimer : permet de supprimer l'objet
    - _consulter : permet de consulter l'objet

- exemple om_droit d'om_utilisateur

    - om_utilisateur = 5 en form.php?obj=om_utilisateur : accès integrale aux utilisateurs de niveau 5 et plus
    - om_utilisateur_tab = 4 en tab_php?obj=om_utilisateur : accès en lecture de table qu aux utilisateurs de niveau 4 et plus

- chaque profil a acces a tous les droits des profils d' 'un niveau inférieur

- l'administrateur a acces à tout.


=====================
La multi-collectivité
=====================

Les collectivités peuvent être de niveau 1 ou de niveau 2. Les utilisateurs de chaque collectivité heritent de ce niveau.
Les utilisateurs de niveau 1 n'ont accès qu'a leur collectivité tandis que les utilisateurs de niveau 2 ont accès à toutes les collectivités disponibles.
Lors de la conception de la base de données un champ om_collectivite peut être ajouté à chaque table ayant besoin d'un filtrage par collectivité.
Les utilisateurs de niveau 1 ne veront aucune notion de collectivité et n'auront accès qu'aux éléments liés à la leur.



===================
Les login et logout
===================

Le login se fait par le script *scr/login.php*

login.php valorise les variables sessions  permettant la gestion des acces et securites


  .. code-block:: php

    <?php
      $_SESSION['profil'] = $profil;
      $_SESSION['nom'] = $nom;
      $_SESSION['login'] = $login;
    ?>

La deconnexion se fait avec le script  *scr/logout*

Le changement de mot de passe se fait avec le script  *scr/password.php*

L'accès au changement de passe se fait par défaut dans le menu haut
(voir framework/paramétrage)


===============
Les utilitaires
===============

La gestion des droits d'acces se fait dans les méthodes des utilitaires

    php/openmairie/om_appication.class.php (composant openMairie)

    obj/utils.class.php
    
(:ref:`voir framework/utilitaire<utilitaire>`)
