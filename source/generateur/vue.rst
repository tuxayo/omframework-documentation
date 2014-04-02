.. _vue:

########
Les vues
########

Les vues ne sont utilisables qu'avec postgresql et elles apparaissent dans la grille d'affichage des objets du générateur dans le
fieldset "vues".

Il est possible d'utiliser les vues pour faire des formulaires, états, requetes mémorisés ...
de la même manière que les tables sauf au niveau de la mise a jour (il faut paramétrer la vue de manière particulière)

Il est possible d'utiliser dblink pour créer une vue dans une base externe. 

Les vues ont été initiées dans la version 4.1.0.


===========
vue interne
===========

creation d'une vue en sql ::

    CREATE OR REPLACE VIEW om_administrateur AS 
    SELECT om_utilisateur.om_utilisateur AS om_administrateur, om_utilisateur.nom,
    om_utilisateur.login, om_utilisateur.om_profil
    FROM om_utilisateur
    WHERE om_utilisateur.om_profil::text = '5'::text;


=========================
vues externes avec dblink
=========================

install dblink
==============
ubuntu : pakage postgresql 8.X ou 9.X contrib  dans synoptic

Dans la base cible, l'installation des fonctions dblink se fait en executant la requête
var/share/postgresql/8.X ou 9.X/contrib/dblink.sql

test sql ::

    SELECT dossier,nature FROM dblink('dbname=openfoncier','SELECT dossier,nature FROM
        dossier where nature = \'PC\'') as (dossier varchar(11), nature char(2))

Attention, il vaut mieux ne pas mettre les chaines de connexion dans les fichiers de
parametrage openMairie


Creation de vue externe
=======================

en utilisant dblink ::

    -- creation de vues sur un même serveur 

    CREATE VIEW openboisson_etablissement AS
       SELECT *
            FROM dblink('dbname=openboisson','SELECT etablissement, raison_sociale FROM
            etablissement') as (openboisson_etablissement integer , 
                                raison_sociale varchar(30));

    

 
========================================================
Problème à régler dans l'utilisation d une vue externe
========================================================

-  il faut utiliser les fonctions d'encodage de pgsql si les 2 bases n'ont pas le
   même encodage

- il faut utiliser une sequence externe ou interne en insert 

- il faut vérifier la cle secondaire dans la base ou schéma d'origine


    
- attention : la creation de vue non opérationnelle fait dysfonctionner le générateur qui fait appel au catalogue de vue : select viewname from pg_views

