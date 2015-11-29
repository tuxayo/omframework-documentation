.. _base:

==========================================
Integration de la version 4.4.x en 4.4.5 :
==========================================

Les principes de la verion om4.5.x sont les suivants :

- cohérence de la liaison de un à plusieurs : om_sig / maj de champs

- utilisation d'om_sig dans le moteur de recherche et reqmo

- création d une table d'extension remplacant var_sig.inc

- la table de flux remplace celle des wms car il y a plusieurs type de flux

Pour faire évoluer la base, utiliser data/pgsql/ver_4.4.5.sql

Il est décrit ici les modifications de la base :

Nouveaux champs om_sig_map ::

    champ_idx character varying(30);
    util_idx boolean;
    util_reqmo boolean;
    util_recherche boolean;
    source_flux integer;
    fond_default character varying(10); Paramétrage du champ par défaut

    sld_marqueur character varying(254);
    sld_data character varying(254);
    point_centrage geometry(Point,2154),;


Dans om_sig_map_comp, sont intégrés la partie mise à jour d om_sig_map (respect du modèle référentiel)

Ces colonnes relatives à  la mise à jour sont supprimées dans om_sig_map::

    lib_geometrie;
    maj;
    table_update;
    champ_idx;
    champ;
    type_geometrie;

Création d'une table des extensions (remplaçant les variables de dyn/var_sig.inc.php) et clé secondaire
dans om_sig_map::

	om_sig_extent integer NOT NULL,
	nom character varying(150),
	extent character varying(150)
    
Creation d une table om_sig_flux remplacant om_sig_wms car les flux peuvent être différents que wms ::

    om_sig_flux integer NOT NULL,
    libelle character varying(50) NOT NULL,
    om_collectivite integer NOT NULL,
    id character varying(50) NOT NULL,
    attribution character varying(150),
    chemin character varying(255) NOT NULL,
    couches character varying(255) NOT NULL,
    cache_type character varying(3),
    cache_gfi_chemin character varying(255),
    cache_gfi_couches character varying(255)

Dans le modèle, remplacement de tous les champs oui/non par des types booleans