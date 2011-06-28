.. _vue:

########
Les vues
########

Les vues ne sont utilisables qu avec postgresql.

Il est possible d'utiliser les vues pour faire des formulaires, états, requetes mémorisés ...
de la même manière que les tables sauf au niveau de la mise a jour ou il faut mettre
un trigger


Il est possible d'utiliser dblink pour créer une vue dans une base externe 

===========
vue interne
===========

creation d'une vue en sql ::

   
    CREATE OR REPLACE RULE om_administrateur_insert AS ON INSERT TO om_administrateur
    DO INSTEAD
    INSERT INTO om_utilisateur (om_utilisateur=new.administrateur, nom=new.nom,
    login=new.login, om_profil=new.om_profil)
    


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




Creation de vue externe
=======================

en utilisant dblink sur un même serveur ::

    CREATE VIEW openboisson_etablissement AS
       SELECT *
            FROM dblink('dbname=openboisson','SELECT etablissement, raison_sociale FROM
            etablissement') as (openboisson_etablissement integer , 
                                raison_sociale varchar(30));


    -- creation de vue sur serveur distant ( a voir)

    CREATE VIEW chios_openboisson_etablissement AS
       SELECT *
            FROM dblink('hostaddr=chios port=5432 dbname=openboisson user=postgresql password=postgresql',
            'SELECT etablissement, raison_sociale FROM
            etablissement') as (openboisson_etablissement integer , 
                                raison_sociale varchar(30));




========================
Mise a jour dans une vue
========================

Pour qu'une vue soit  modifiable,  indépendamment  des contraintes d'intégrité sur
les tables, il faut respecter un certain nombre de règles ::
 

    * Pas de directives distinct
    * Pas de fonctions d'agrégat (AVG, COUNT …)
    * PAS de GROUP BY, ORDER BY, HAVING
    * La vue ne doit pas être déclarée en lecture seule (WITH READ ONLY)



En  mis a jour une erreur apparait ::

    -- en base interne ou base externe

    UPDATE public.om_administrateur SET om_administrateur = '1',
        nom = 'ADMINISTRATEUR',login = 'admin',om_profil = '5' WHERE om_administrateur = 1
    SGBD: nativecode=ERREUR: ne peut pas mettre a  jour une vue HINT:
    Vous avez besoin d'une regle non conditionnelle ON UPDATE DO INSTEAD


Pour mettre à jour vous devez créer un trigger ::


en mise a jour interne ::

    -- mise a jour

    CREATE OR REPLACE RULE om_administrateur_update AS
       ON UPDATE TO om_administrateur
       DO INSTEAD 
        UPDATE om_utilisateur SET nom = new.nom, login = new.login, om_profil = new.om_profil
        WHERE om_utilisateur.om_utilisateur = old.om_administrateur;
    
      old ou new ???
     
    -- destruction

    CREATE OR REPLACE RULE om_administrateur_delete AS
       ON DELETE TO om_administrateur
       DO INSTEAD 
        DELETE from om_utilisateur 
        WHERE om_utilisateur.om_utilisateur = old.om_administrateur;


en mise a jour externe ::


    CREATE OR REPLACE RULE openboisson_etablissement_update AS
       ON UPDATE TO openboisson_etablissement
       DO INSTEAD
       dblink('dbname=openboisson','update etablissement set raison_sociale new.openboisson_raison_sociale
          WHERE etablissement.etablissement = old.openboisson_etablissement')

    -- ne fonctionne pas



 
========================================================
Problème non réglés dans l'utilisation d une vue externe
========================================================

- problème d encodage si les 2 bases ne sont pas encodés de la même manière
l'encodage est celui de a base applicative.

- utilisation d une sequence externe ou interne en creation 

- verification de cle secondaire dans la base d origine non preis en compte dans
le base cible


- attention : la creation de vue qui ne fonctionne pas fait dysfonctionner le
generateur qui fait appel au catalogue de vue : select viewname from pg_views
Mettre en place un code erreur qui n execute pa l UNION ?