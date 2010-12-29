 .. _acces:

####################
La gestion des accès
####################



Le framework fournit un gestionnaire d'accés accessible dans le menu à :

- administration -> profil

- administration -> droit

- administration ->utilisateur

Les accès sont conservés dans des tables.

==========
Les tables
==========

La gestion des accès est gérée avec 3 tables :

**om_profil** : gestion des profils ::

    administrateur
    super utilisateur
    utilisateur
    utilisateur limite
    consultation

**om_droit**: la gestion des droits affecte un profil suivant chaque :

    objet métier : $obj om_collectivite, om_parametre ...

    chaque rubrique du menu :

    voir paramétrage menu : tableau Rubrik  right = "om_parametre"
            

**om_utilisateur** : cette table permet de donner un login, un mot de passe
et un profil à chaque utilisateur

    
    
Diagramme de classe

.. image:: ../_static/acces_1.png

==========
Les règles
==========

- le droit sur un objet porte le nom de l'objet

- chaque profil a acces a tous les droits des profils d un niveau inférieur

- l'adminitrateur a acces à tout.


===================
Les login et logout
===================

Le login se fait par le script *scr/login.php*

login.php valorise les variables sessions  permettant la gestion des acces et securites::

      $_SESSION['profil'] = $profil;
      $_SESSION['nom'] = $nom;
      $_SESSION['login'] = $login;

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
    
(*voir framework/utilitaire*)
