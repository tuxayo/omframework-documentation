.. _vue:

########
Les vues
########

Les vues ne sont utilisables qu'avec postgresql.

Il est possible d'utiliser les vues pour faire des formulaires, états, requetes mémorisés ...
de la même manière que les tables sauf au niveau de la mise a jour ou il faut mettre
un trigger.

Il est possible d'utiliser dblink pour créer une vue dans une base externe. 

Les vues ont été initiées dans la version 4.1.0. Elles peuvent être interne (dans une même base) ou externe en utilisant dblink. 


===========
vue interne
==========

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


    -- creation de vues sur serveur distant ( a voir)

    CREATE VIEW chios_openboisson_etablissement AS
       SELECT *
            FROM dblink('hostaddr=10.1.0.1 port=5432 dbname=openboisson user=postgresql password=postgresql',
            'SELECT etablissement, raison_sociale FROM
            etablissement') as (openboisson_etablissement integer , 
                                raison_sociale varchar(30));

    -- possibilité de créer des connect permanent et gerer des alias
    

========================
Mise a jour dans une vue
========================

Pour qu'une vue soit  modifiable,  indépendamment  des contraintes d'intégrité sur
les tables, il faut respecter un certain nombre de règles :: 

    Pas de directives distinct
    Pas de fonctions d'agrégat (AVG, COUNT …)
    PAS de GROUP BY, ORDER BY, HAVING
    La vue ne doit pas être déclarée en lecture seule (WITH READ ONLY)


Pour mettre à jour vous devez créer un trigger 

en vue interne ::

    -- mise a jour

    CREATE OR REPLACE RULE om_administrateur_update AS
       ON UPDATE TO om_administrateur
       DO INSTEAD 
        UPDATE om_utilisateur SET nom = new.nom, login = new.login, om_profil = new.om_profil
        WHERE om_utilisateur.om_utilisateur = old.om_administrateur;

    -- destruction

    CREATE OR REPLACE RULE om_administrateur_delete AS
       ON DELETE TO om_administrateur
       DO INSTEAD 
        DELETE from om_utilisateur 
        WHERE om_utilisateur.om_utilisateur = old.om_administrateur;

    CREATE OR REPLACE RULE om_administrateur_insert AS ON INSERT TO om_administrateur
    DO INSTEAD
    INSERT INTO om_utilisateur (om_utilisateur=new.administrateur, nom=new.nom,
    login=new.login, om_profil=new.om_profil)


en vue externe ::

    -- Il ne semble pas y avoir la possibilité de mettre une regle
    -- ne serait il pas possible d executer d une procedure stockee a partir de instead (declencheur)

    CREATE OR REPLACE RULE openboisson_etablissement_update AS
       ON UPDATE TO openboisson_etablissement
       DO INSTEAD
       dblink_exec('dbname=openboisson','update etablissement set raison_sociale new.openboisson_raison_sociale
          WHERE etablissement.etablissement = old.openboisson_etablissement')
    
    -- dblink_exec ne fonctionne pas (a valider)

    -- par contre la requete ci dessous marche
    
    SELECT dblink_exec('dbname=openboisson',
          'update etablissement set raison_sociale = ''zzz'' where etablissement =3;');
  
    -- il serait donc possible de modifier la methode modifier en surchargeant obj/openboisson_etablissement.class.php
    
    function modifier($val = array(), &$db = NULL, $DEBUG = false) {
    ...
            $this->triggermodifier ...
            // MODIFS ==========================================
            $sql="SELECT dblink_exec('dbname=openboisson',
                 'update etablissement set raison_sociale = ''".
                 $this->valF['raison_sociale'].
                 "'' where etablissement = ".$id."')";
            $res=$db->query($sql); 
            // FIN MODIFS =======================================
            if (database::isError ....
 
    }


 
========================================================
Problème à régler dans l'utilisation d une vue externe
========================================================

-  il faut utiliser les fonctions d'encodage de pgsql si les 2 bases n'ont pas le
   même encodage

- il faut utiliser une sequence externe ou interne en insert 

- il faut vérifier la cle secondaire dans la base d origine

- dans qgis pour utiliser une vue :
    - regarder la table 'geometry_columns'" ne soit pas cochée (option editer)
    - modifier la clé primaire dans la liste des tables de connexion 
    
- attention : la creation de vue non opérationnelle  fait dysfonctionner le
generateur qui fait appel au catalogue de vue : select viewname from pg_views


======================
Les procedure stockées
======================

operation qui double un entier ::

    CREATE FUNCTION om_double (integer) 
      RETURNS integer 
      AS 
    '
      BEGIN 
        RETURN 2*$1;
      END; 
    '
    LANGUAGE 'plpgsql';

    -- lancement
    select om_double(2);
    
recuperer un tarif d'une occupation::
    
    CREATE FUNCTION om_get_tarif (INTEGER)
    RETURNS FLOAT
    AS
    '
      DECLARE 
        montant FLOAT;
      BEGIN
        SELECT INTO montant  tarif FROM occupation WHERE occupation= $1;
        RETURN montant;
      END ;
    '
    LANGUAGE 'plpgsql';
     
    -- lancement
    select om_get_tarif(1);

compter les etablissements ::

    CREATE OR REPLACE FUNCTION om_compte_etablissement() RETURNS INTEGER AS 
    '
    DECLARE
    count INTEGER;
    myrec RECORD;
    BEGIN
    FOR myrec IN SELECT * FROM DBLINK('dbname=openboisson',
                        'select etablissement from etablissement') as
                        temp(etablissement integer)
    LOOP
        count := count + 1;
    END LOOP;
    RETURN count;
    END; ' LANGUAGE 'plpgsql';

    -- ne renvoie rien
