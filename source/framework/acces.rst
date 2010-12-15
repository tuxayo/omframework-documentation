.. _framework:

#################
gestion des acces
#################

====
menu
====

administration  -> profil
                -> droit
                ->utilisateur

==========
Les tables
==========

La gestion des accès est gérée avec 3 tables :

om_profil : gestion des profils
    administrateur
    super utilisateur
    utilisateur
    utilisateur limite
    consultation

om_droit : gestion des droits suivant chaque :
    objet métier : $obj om_collectivite, om_parametre ...
    rubrique du menu :
        Rubrik
            right = ...
            
om_utilisateur : gestion des utilisateurs

==============
Fonctionnement
==============

un profil est attribué à chaque utilisateur
    des droits sont affectés = chaque profil
    le droit d'un objet porte le nom de l'objet

==========
contrainte
==========

chaque profil a acces a tous les droits des profils d un niveau plus bas
l'adminitrateur a acces a tout.

=====
login
=====
scr/login.php

login.php valorise les variables sessions  permettant la gestion des acces et securites:

      $_SESSION['profil'] = $profil;
      $_SESSION['nom'] = $nom;
      $_SESSION['login'] = $login;

scr/logout

scr/password.php  changement de mot de passe

methodes
    php/openmairie/om_appication.class.php (composant openMairie)
    obj/utils.class.php