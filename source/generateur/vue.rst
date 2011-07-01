.. _vue:

########
Les vues
########

Les vues ne sont utilisables qu'avec postgresql.

Il est possible d'utiliser les vues pour faire des formulaires, états, requetes mémorisés ...
de la même manière que les tables sauf au niveau de la mise a jour ou il faut mettre
un trigger.

Il est possible d'utiliser dblink pour créer une vue dans une base externe. 

Dans la version 4.1.0, les vues sont initiées a titre expérimental.


===========
vue interne
===========

creation d'une vue en sql ::

    CREATE OR REPLACE VIEW om_administrateur AS 
    SELECT om_utilisateur.om_utilisateur AS om_administrateur, om_utilisateur.nom,
    om_utilisateur.login, om_utilisateur.om_profil
    FROM om_utilisateur
    WHERE om_utilisateur.om_profil::text = '5'::text;
   

    

exemple utilisation new et old utile dans le cadre de la mise a jour d un montant ::
    old valeur d un montant =200
    new valeur d'un montant =300
    si operation +100 avec possibilité de refuser


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
        $id = $val[$this->clePrimaire];
        $this->setValF($val);
        $this->verifier($val, $db, $DEBUG);
        $this->testverrou();
        if ($this->correct) {
             $this->triggermodifier($id, $db, $val, $DEBUG);
            // MODIFS ==========================================
            $sql="SELECT dblink_exec('dbname=openboisson',
                 'update etablissement set raison_sociale = ''".
                 $this->valF['raison_sociale'].
                 "'' where etablissement = ".$id."')";
            $res=$db->query($sql); 
            // FIN MODIFS =======================================
            if (database::isError($res)) {
                $this->erreur_db($res->getDebugInfo(), $res->getMessage(), '');
            } else {
                $this->addToLog(_("Requete executee"), VERBOSE_MODE);
                $message = _("Enregistrement")."&nbsp;".$id."&nbsp;";
                $message .= _("de la table")."&nbsp;\"".$this->table."\"&nbsp;";
                // PROBLEME affectedRows ne fonctionne pas avec dblink
                // $message .= "&nbsp;".$db->affectedRows()."&nbsp;";
                $message .= _("enregistrement(s) mis a jour")."&nbsp;";
                $this->addToLog($message, VERBOSE_MODE);
                // PAS AFFECTE 
                //if ($db->affectedRows() == 0) {
                //    $this->addToMessage(_("Attention vous n'avez fait aucune modification.")."<br/>");
                //} else {
                //    $this->addToMessage(_("Vos modifications ont bien ete enregistrees.")."<br/>");
                //}
                $this->verrouille();
            }
            $this->triggermodifierapres($id, $db, $val, $DEBUG);
        } else {
            $this->addToMessage("<br/>"._("SAISIE NON ENREGISTREE")."<br/>");
        }
    }


 
========================================================
Problème non réglés dans l'utilisation d une vue externe
========================================================

- problème d encodage si les 2 bases ne sont pas encodés de la même manière
l'encodage est celui de a base en cours.

- utilisation d une sequence externe ou interne en insert ::

    -- en externe il apparait dangereux de faire un insert 
  
    -- en interne, on peut surcharger openboisson_etablissement.class.php::
  
    function setId(&$db) {
      //numero automatique
          $this->valF[$this->table] = $db->nextId(DB_PREFIXE."om_utilisateur");
    }


- verification de cle secondaire dans la base d origine n'est pas pris en compte par openMairie dans
le base cible. La protection des clés se fait dans la base cible par postgresql
mais le message d erreur n'est pas inteprété par openMairie.

- utilisation dans qgis d une vue : Dans QGis: pour que ta vue apparaisse dans la liste
il faut que "uniquement regarder la table 'geometry_columns'" ne soit pas cochée)
(dans editer) selectionner la clé primaire (dans la liste des tables de connexion
modifier la colone de la clé primaire) avant de cliquer sur le bouton ajouter ... 

- attention : la creation de vue qui ne fonctionne pas fait dysfonctionner le
generateur qui fait appel au catalogue de vue : select viewname from pg_views
Mettre en place un code erreur qui n execute pa l UNION ?


==================
procedure stockées
==================

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




