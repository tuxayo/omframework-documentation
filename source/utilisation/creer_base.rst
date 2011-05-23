.. _creer_base:


########################
Créer la base de données
########################

Vous devez au préalable copier openmairie_exemple dans le repertoire www de votre serveur apache


Il vous est proposé de créer la base de données sous mysql :

- Créer une base de données appellée "openmairie"

- Créer les tables nécessaires au framework openMairie avec le fichier sql
    
    data/mysql/init.sql


- Créer les tables necessaires a notre exemple



    - table courrier ::
    
        courrier        int 8       cle primaire
        dateenvoi       date
        objetcourrier   text
        emetteur        int8        cle secondaire
        service         int8        cle secondaire
    
    
    - table emetteur ::
    
        emetteur        int 8       cle primaire
        nom             varchar 20
        prenom          varchar 20

        
    - table service ::
    
        service         int 8        cle primaire
        libelle         varchar 20



- modifier le paramétrage openMairie pour faire un accès à la base créée si votre base a un nom différent d'openMairie :


    dyn/database.inc.php

    voir framework/parametrage



- accéder avec votre navigateur sur openmairie_exemple ::

    login : demo
    mot de passe : demo


Le script mysql de création de la base de l'exemple est le suivant ::


    --
    -- Structure de la table 'courrier'
    --
    
    CREATE TABLE courrier (
      courrier int(8) NOT NULL,
      dateenvoi date NOT NULL,
      objetcourrier text NOT NULL,
      emetteur int(8) NOT NULL,
      service int(8) NOT NULL,
      PRIMARY KEY  (courrier)
    ) TYPE=MyISAM;
    
    --
    -- Structure de la table 'emetteur'
    --
    
    CREATE TABLE emetteur (
      emetteur int(8) NOT NULL,
      nom varchar(20) NOT NULL,
      prenom varchar(20) NOT NULL,
      PRIMARY KEY  (emetteur)
    ) TYPE=MyISAM;
    
    --
    -- Structure de la table 'service'
    --
    
    CREATE TABLE service (
      service int(8) NOT NULL,
      libelle varchar(20) NOT NULL,
      PRIMARY KEY  (service)
    ) TYPE=MyISAM;