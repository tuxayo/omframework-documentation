.. _creer_base:

- Copier openmairie_exemple dans le repertoire www de votre serveur apache
- Donner les droits d'ecriture sur les repertoires /gen et /sql (linux)

########################
creer la base de données
########################


Il vous est proposé de créer la base de données sous mysql :

- Creer une base de donnee appellée "openmairie"

- Creer les tables necessaires au framework openMairie avec le fichier sql
    
    data/mysql/init.sql


- Creer les tables necessaires a notre exemple


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
        
    - table service
    
        service         int 8        cle primaire
        libelle         int 8
        

- modifier le paramétrage openMairie pour faire un acces à la base créée
si votre base a un nom différent d'openMairie :

    dyn/database.inc.php

- acceder avec votre navigateur sur openmairie_exemple :

    login : demo
    mot de passe : demo
