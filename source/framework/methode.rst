.. _methode:

##########
La méthode
##########

Il est décrit ici la méthode pour la création d' objets métiers:

Le développement consiste à créer des objets métier (/obj) qui surchargent
la classe abstraite  om_dbformdyn.class.php et à modifier les valeurs par défaut
des variables dans les fichiers sql (nom_objet.inc et nom_objet.form.inc)


Voir aussi le *générateur* pour automatiser les scripts métier.



=================================
Surcharger les classes openMairie
=================================


Il vaut mieux utiliser le générateur pour initialiser les classes metiers.

Le générateur surcharge la classe om_dbformdyn.class.php par rapport aux informations de la base ::

    classe abstraite <- classe metier generee <- classe metier 1 <- classe metier 2 ...
    openMairie             depuis la base
    

    om_dbformdyn.class.php <- gen/obj/nom_objet.class.php <- obj/nom_objet.class.php



Exemple avec concession d'openCimetiere ::

    om_dbformdyn.class.php
            <- gen/obj/emplacement.class.php
                <-/obj/emplacement.class.php <- /obj/concession.class.php



===============================
Modifier les valeurs par défaut
===============================

Il est décrit ici les valeurs par défaut dans core/om_dbformdyn.class.php 
qui est une classe d'openMairie.


Les valeurs suivantes sont mises par defaut afin de pouvoir construire rapidemment un formulaire ::

    valeur par defaut  
       en ajout = initialisation vide
   
    type par defaut
       type text pour ajout et modification
       type hiddenstatic pour suppression
   
    libelle par défaut :
       Libellé = nom du champ dans le SGBD
   
    taille et max d un champ
       Taille et max = longueur du champ dans le SGBD
   
    les regroupements et groupements de champs sont vides
   
    les fonctions javascript ne sont pas utilisées

 
===========================================================
Modifier les valeurs par defaut par les méthodes assesseurs
===========================================================

Elles se font dans la classe obj/nom_objet.class.php

Les valeurs par défaut sont modifiées par la méthode setVal(nomduchamp, nouvelle valeur)

Les types par défaut sont modifiés par la méthode setType(nomduchamp, nouveau type)

Les longueurs d affichage par défaut sont modifiées par la méthode setTaille(nomduchamp, nouvelle valeur)

Les maximums autorisés par défaut sont modifiés par la méthode setMax(nomduchamp, nouvelle valeur)

Les libelles de champ par défaut sont modifiés par la méthode setLib(nomduchamp, nouvelle valeur)

Les scripts javascript sont appellés dans la méthode setOnchange()


Voir framework/formulaire

===============================
La class om_dbformdyn.class.php
===============================

om_dbform.class.php  est une classe openMairie dans core/

La classe abstraite dbform gère l’interface entre l'objet métier et la base de données connectée via DBPEAR.

Les méthodes principales sont les suivantes :

* orientées sgbd ::

    constructeur
    ajouter : Ajoute un objet
    Modifier : Modifie un objet
    Supprimer : Supprime un objet
    Verifier : Contrôle un objet
    Clesecondaire : Contrôle les cles secondaires
    triggers avant/apres ajout/modification/suppression

* orientees Formulaire ::

    Formulaire : Constitue le formulaire et fait appel à formulaire.dyn.class.php
    sousFormulaire : Constitue le sousformulaire -> appel à formulaire.dyn.class.php
    Message : Retourne le message d erreur (contrôle php)
    bouton : Affiche le bouton
    Retour : gére le retour à une interface php en fin de saisie
    sousformulaireRetour : gére le retour à une interface php en fin de saisie de sous formulaire
    setType : Envoi au formulaire les type de champ
    setVal : Envoi au formulaire les valeurs par défaut
    setValSousformulaire : Envoi au sousformulaire les valeurs par défaut
    setlib : Envoi au formulaire les libellés de champs
    setTaille : Envoi au formulaire la taille du champ
    setMax : Envoi au formulaire la taille maximum autorisée du champ
    setSelect : Envoi au formulaire les champs select à afficher
    setOnchange : Envoi au formulaire les controles javascript à effectuer en cas de changement de données dans le champ
    setGroupe : Envoi au formulaire le regroupement de champ par ligne
    setRegroupe : Envoi au formulaire un fieldset
    setOnkeyup
    setOnclick
    mail
    selectiste
    selectlistemulti

* des fonctions de traitement de champ heure et date::

    DateDB : transforme les dates affichées en date pour base de données
    HeureDB : controle du champs heure saisi 00 ou 00:00 ou 00:00:00
    DateSystemeDB : mise au format base de donnees de la date systeme
    DatePHP : controle et transforme la date saisie (jj/mm/aaaa) en date format PHP

*  des fonctions pour faire des calculs ::

    AnneePHP : controle et recupere l’année de la date saisie (jj/mm/aaaa)
    MoisPHP : controle et recupere le mois de la date saisie (jj/mm/aaaa)
    JourPHP : controle et recupere le jour de la date saisie (jj/mm/aaaa)

La classe dbformdyn.class.php fait appel à la classe formulaire.dyn.class.php pour afficher le formulaire.

Il est créé 2 objets :

- un objet db qui fait la connexion avec la base

- un objet form qui décrit le formulaire


==========
L'objet db
==========

db est l'objet de connexion a la base dont les proprietes sont les suivantes ::

    DB_pgsql Object
    
    (
    [phptype] => pgsql 
	[dbsyntax] => pgsql 
	[features] => Array ( 
			[limit]	=> alter 
			[new_link] => 4.3.0 
			[numrows] => 1 
			[pconnect] => 1 
			[prepare] => 
			[ssl] => 1 
			[transactions] => 1 ) 
			[errorcode_map] => Array ( ) 
			[connection] => Resource id #19 
			[dsn] => Array ( 
				[phptype] => pgsql 
				[dbsyntax] => pgsql 
				[username] => postgres 
				[password] => postgres 
				[protocol] => tcp 
				[hostspec] => localhost 
				[port] => 5432 
				[socket] => 
				[database] => sig 
				[title] => Openmairie Exemple PostGreSQL schema SIG 
				[formatdate] => AAAA-MM-JJ 
				[schema] => openmairie 
			) 
			[autocommit] => 1 
			[transaction_opcount] => 0 
			[affected] => 0 
			[row] => Array ([20] => 10 ) 
			[_num_rows] => Array ( [20] => 10 ) 
			[fetchmode] => 1 
			[fetchmode_object_class] => stdClass 
			[was_connected] => 
			[last_query] => select * from openmairie.om_parametre where om_collectivite=2 
			[options] => Array (
                [result_buffering] => 500 
				[persistent] => 
				[ssl] => 
                [debug] => 2 
                [seqname_format] => %s_seq 
                [autofree] => 
                [portability] => 63 
                [optimize] => performance 
                )
			[last_parameters] => Array ( ) 
			[prepare_tokens] => Array ( ) 
			[prepare_types] => Array ( ) 
			[prepared_queries] => Array ( ) 
			[_last_query_manip] => 
			[_next_query_manip] => 
			[_debug] => 
			[_default_error_mode] => 
			[_default_error_options] => 
			[_default_error_handler] => 
			[_error_class] => DB_Error 
			[_expected_errors] => Array ( ) 
    )
    
============
L'objet form
============

form est l'objet formulaire dont les proprietes sont les suivantes ::
  
    formulaire Object (
        [enteteTab] =>
        [val] => Array (
                [om_parametre] => 1
                [libelle] => maire
                [valeur] => O PENMAIRIE
                [om_collectivite] => 1 )
        [type] => Array (
                [om_parametre] => text
                [libelle] => text
                [valeur] => text
                [om_collectivite] => text )
        [taille] => Array (
                [om_parametre] => 11
                [libelle] => 20
                [valeur] => 50
                [om_collectivite] => 11 )
        [max] => Array (
                [om_parametre] => 11
                [libelle] => 20
                [valeur] => 50
                [om_collectivite] => 11 )
        [lib] => Array (
                [om_parametre] => Om_parametre
                [libelle] => Libelle
                [valeur] => Valeur
                [om_collectivite] => Om_collectivite )
        [groupe] => Array (
                [om_parametre] =>
                [libelle] =>
                [valeur] =>
                [om_collectivite] => )
        [select] => Array (
                [om_parametre] =>  Array ([0] => [1] => )
                [libelle] => Array ( [0] => [1] => )
                [valeur] => Array ( [0] => [1] => )
                [om_collectivite] => Array ( [0] => [1] => ) )
        [onchange] => Array (
                [om_parametre] =>
                [libelle] =>
                [valeur] =>
                [om_collectivite] => )
        [onkeyup] => Array (
                [om_parametre] =>
                [libelle] =>
                [valeur] =>
                [om_collectivite] => )
        [onclick] => Array (
                [om_parametre] =>
                [libelle] =>
                [valeur] =>
                [om_collectivite] => )
        [regroupe] =>
        [correct] =>
    ) 
       