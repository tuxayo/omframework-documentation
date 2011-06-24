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


    CREATE VIEW om_administrateur AS
        SELECT om_utilisateur as om_administrateur, nom,login,om_profil
            FROM om_utilisateur
            WHERE om_profil = '5'
     
    CREATE OR REPLACE RULE om_update AS ON UPDATE TO om_administrateur
    DO INSTEAD
    UPDATE om_utilisateur SET nom=new.nom, login=new.login, om_profil=new.om_profil 
    WHERE om_utilisateur=new.om_administrateur
    
    -> creer une regle _return / a voir nom
       pas possible de supprimer la regle
    
    CREATE OR REPLACE RULE om_administrateur_insert AS ON INSERT TO om_administrateur
    DO INSTEAD
    INSERT INTO om_utilisateur (om_utilisateur=new.administrateur, nom=new.nom,
    login=new.login, om_profil=new.om_profil)
    
    CREATE OR REPLACE RULE om_administrateur_insert AS ON INSERT TO view_name
    DO INSTEAD
    INSERT INTO table_name VALUES(id=new.id,name=new.name)
    


    III) for Delete
    
    CREATE OR REPLACE RULE om_administrateur_delete AS ON DELETE TO om_administrateur
    DO INSTEAD
    DELETE FROM om_utilisateur WHERE om_utilisateur=new.om_administrateur


 Créer une vue modifiable 

Pour qu'une vue soit  modifiable,  indépendamment  des contraintes d'intégrité sur
les tables, il faut respecter un certain nombre de règles ::
 

    * Pas de directives distinct
    * Pas de fonctions d'agrégat (AVG, COUNT …)
    * PAS de GROUP BY, ORDER BY, HAVING
    * La vue ne doit pas être déclarée en lecture seule (WITH READ ONLY)




========================
vues eternes avec dblink
========================

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

    CREATE VIEW openfoncier_dossier AS
      SELECT *
        FROM dblink('dbname=openfoncier','SELECT dossier,nature FROM
        dossier  where nature = ''PC''') as (openfoncier_dossier varchar(12) , openfoncier_nature char(2))
    
    
    CREATE VIEW openfoncier_nature AS
        SELECT *
        FROM dblink('dbname=openfoncier','SELECT nature,libelle FROM
            nature') as (openfoncier_nature char(2) , libelle varchar(30))
    
    CREATE VIEW etablissement AS
      SELECT etablissement,raisonsociale,exploitant
        FROM dblink('dbname=openboisson','SELECT * FROM
        etablissement') as (etablissement integer , raisonsociale varchar(30),exploitant integer)


========================
Mise a jour dans une vue
========================

Normalement une vue n'est pas faite pour etre mis a jour et une erreur apparait ::

    en base interne

    UPDATE public.om_administrateur SET om_administrateur = '1',nom = 'ADMINISTRATEUR',login = 'admin',om_profil = '5' WHERE om_administrateur = 1
    SGBD: nativecode=ERREUR: ne peut pas mettre a  jour une vue HINT: Vous avez besoin d'une rÃ¨gle non conditionnelle ON UPDATE DO INSTEAD

    en base externe

    UPDATE public.openfoncier_dossier SET openfoncier_dossier = 'PC11R0033',nature = 'PA' WHERE openfoncier_dossier = 'PC11R0033'
    SGBD: nativecode=ERREUR: ne peut pas mettre a  jour une vue HINT: Vous avez besoin d'une rÃ¨gle non conditionnelle ON UPDATE DO INSTEAD.
    DB Error: unknown error


Vous devez créer un trigger ::

    --Create an INSTEAD OF INSERT trigger on the view.
    CREATE TRIGGER InsteadTrigger on InsteadView
    INSTEAD OF INSERT
    AS
    BEGIN
      --Build an INSERT statement ignoring inserted.PrimaryKey and
      --inserted.ComputedCol.
      INSERT INTO BaseTable
           SELECT Color, Material
           FROM inserted
    END
    GO
    
    
    CREATE OR REPLACE TRIGGER t_vproduct
    INSTEAD OF UPDATE
    ON v_product
 
================================================
Problème non réglés dans l'utilisation d une vue
================================================

- problème d encodage si les 2 bases ne snt pas encodés de la même manière
L encodage est celui de a base applicative.

- utilisation d une sequence externe 