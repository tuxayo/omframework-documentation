 .. _acces:

####################
La gestion des accès
####################



Le framework fournit un gestionnaire d'accés accessible dans le menu à ::

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

- le droit sur un objet porte le nom de l'objet, avec l'extension _tab, il porte sur l'affichage en table de l'objet

exemple om_droit d'om_utilisateur::

    om_utilisateur = 5      en form.php?obj=om_utilisateur :
                    accès en maj permis au utilisateurs de niveau 5 et plus
    om_utilisateur_tab = 4  en tab_php?obj=om_utilisateur :
                    acces en lecture de table qu aux utilisateurs de niveau 4 et plus

- accès à la rubrique se paramètre dans le menu et dans om_droit

exemple ::

    menu_administration = 3
            cette rubrique n'apparait qu'aux utilisateurs d'om_profil  supérieur ou égal à 3


- chaque profil a acces a tous les droits des profils d un niveau inférieur

- l'administrateur a acces à tout.



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
