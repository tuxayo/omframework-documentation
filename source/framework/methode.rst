.. _methode:

##########
La méthode
##########


Le développement consiste à créer des objets métier (/obj) qui surchargent
la classe abstraite  om_dbformdyn.class.php et à modifier les valeurs par défaut
des variables dans les fichiers sql (nom_objet.inc et nom_objet.form.inc)


Voir aussi le *générateur* pour automatiser les scripts métier.

Il est décrit ensuite les 2 objets : objet de connexion db et l'objet formulaire form.

=================================
Surcharger les classes openMairie
=================================


Il vaut mieux utiliser le générateur pour initialiser les classes métiers.

Le générateur surcharge la classe om_dbformdyn.class.php par rapport aux informations de la base ::

    classe abstraite <- classe métier générée <- classe métier 1 <- classe métier 2 ...
    openMairie             depuis la base
    

    om_dbformdyn.class.php <- gen/obj/nom_objet.class.php <- obj/nom_objet.class.php



Exemple avec concession d'openCimetiere ::

    om_dbformdyn.class.php
            <- gen/obj/emplacement.class.php
                <-/obj/emplacement.class.php <- /obj/concession.class.php

La classe dbformdyn.class.php fait appel à la classe formulaire.dyn.class.php pour afficher le formulaire.

Il est créé 2 objets :

- un objet db qui fait la connexion avec la base

- un objet form qui décrit le formulaire



==========
L'objet db
==========

db est l'objet de connexion a la base dont les propriétés sont les suivantes ::

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

form est l'objet formulaire dont les propriétés sont les suivantes ::
  
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
       