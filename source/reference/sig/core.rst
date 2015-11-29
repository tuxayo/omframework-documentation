.. _core:

==========================
La classe om_map.class.php
==========================

Nouveauté version om 4.4.0sig

Ce module est dans CORE

Une classe intermédiaire est à créer dans obj/om_map.class.php

Actuellement créée, elle a un problème de prise en compte du zoom au départ BUG



Les variables globales
======================

Elles sont définies ci dessous ::

    var f;
	projections
        var defBaseProjection;
        var defDisplayProjection;
	
	paramètres
        var	obj;
        var	idx;
        var	sql_lst_idx;
        var	idx_sel;
        var	popup;
        var	seli;
        var etendue;
        var reqmo;
        var premier;
        var recherche;
        var selectioncol;
        var tricol;
        var advs_id;
        var valide;
        var style;
        var onglet;
        var type_utilisation = '';

	gestion de l'affichage
        var affichageZones= array();

	gestion de l'enregistrement
        var recordMultiComp; true: enregistrement de l'ensemble des champs géométriques ;
            false: enregistrement un par un des champs géométriques (par défaut)
        var recordMode; 1 (par défaut): via form_sig; 2 retour des valeurs dans des
            champs fournis dans le tableau recordFields
        var recordFields = array(); listes des champs retour (même index que comp)
	
	om_sig_map
        var sm_titre;
        var sm_source_flux;
        var sm_zoom;
        var sm_fond_sat;
        var sm_fond_osm;
        var sm_fond_bing;
        var sm_layer_info;
        var sm_fond_default;
        var sm_projection_externe;
        var sm_retour;
        var om_sig_map;
        var sm_url;
        var sm_om_sql;
        var sm_om_sql_idx;
        var sm_restrict_extent;
        var sm_sld_marqueur;
        var sm_sld_data;
        var sm_point_centrage;
	
	champs geom
        var cg_obj_class = array();
        var cg_maj = array();
        var cg_table = array();
        var cg_champ_idx = array();
        var cg_champ = array();
        var cg_geometrie = array();
        var cg_lib_geometrie = array();

	champs flux
        var fl_om_sig_map_flux = array();
        var fl_m_ol_map = array();
        var fl_m_visibility = array();
        var fl_m_panier = array();
        var fl_m_pa_nom = array();
        var fl_m_pa_layer = array();
        var fl_m_pa_attribut = array();
        var fl_m_pa_encaps = array();
        var fl_m_pa_sql = array();
        var fl_m_pa_type_geometrie = array();
        var fl_m_sql_filter = array();
        var fl_m_filter = array();
        var fl_m_baselayer = array();
        var fl_m_singletile = array();
        var fl_m_maxzoomlevel = array();
        var fl_w_libelle = array();
        var fl_w_attribution = array();
        var fl_w_id = array();
        var fl_w_chemin = array();
        var fl_w_couches = array();
        var fl_w_cache_type = array();
        var fl_w_cache_gfi_chemin = array();
        var fl_w_cache_gfi_couches = array();
	
	champs pour fonds de carte externes (OSM, Bing, Google)
        var pebl_http_google;
        var pebl_cle_bing;
        var pebl_cle_google;
        var pebl_zoom_osm_maj;
        var pebl_zoom_osm;
        var pebl_zoom_sat_maj;
        var pebl_zoom_sa;
        var pebl_zoom_bing_maj;
        var pebl_zoom_bing;

	paramètres de style pour la couche marqueur
        var img_maj="../img/punaise_sig.png";
        var img_maj_hover="../img/punaise_hover.png";
        var img_consult="../img/punaise_point.png";
        var img_consult_hover="../img/punaise_point_hover.png";
        var img_w=14;
        var img_h=32;
        var img_click="1.3";multiplicateur hauteur et largeur image cliquee

	gestion des paniers
        var cart_type = array(
            "point" => false,
            "line" => false,
            "polygon" => false
        );
	tableau de la barre du menu d'édition menu (id html, false)
        var edit_toolbar= array(
            "#map-edit-nav" => false, 
            "#map-edit-draw-point" => false, 
            "#map-edit-draw-line" => false,
            "#map-edit-draw-polygon" => false,
            "#map-edit-draw-regular" => false,
            "#map-edit-draw-regular-nb" => false,
            "#map-edit-draw-modify" => false,
            "#map-edit-draw-select" => false,
            "#map-edit-draw-erase" => false,
            "#map-edit-cart" => false,
            "#map-edit-get-cart" => false,
            "#map-edit-draw-record" => false,
            "#map-edit-draw-delete" => false,
            "#map-edit-draw-close" => false
            );  

Les methodes 
============

Les méthodes sont les suivantes


methode d'initialisation de l objet map::

    construct(obj + options) 
        initialisation des propriétés de l objet om_sig

	Récupération du paramétrage de l'objet dans les tables om_sig_map
    et om_sig_map_comp. Préalable à toute utilisation de la classe
	function recupOmSigMap()
        requete om_sig_map + om_sig_extend 

    Génère un tableau (idx, sql_lst_idx) correspondant aux données idx/Reqmo/Recherche
    le tableau est afficher par la classe om_table
	function getSelectRestrict(idx, seli)
    
 	Génère un tableau GeoJson correspondant aux données idx/Reqmo/Recherche
	function getGeoJsonDatas(idx, seli)   

	Génère un tableau GeoJson correspondant au panier cart (n de flux)
    avec la liste des enregistrement lst 
	function getGeoJsonCart(cart, lst)

	Génère un tableau GeoJson correspondant aux données idx/Reqmo/Recherche
	function getGeoJsonMarkers(idx)
    
	calcul des filtres pour les flux de type WMS (fl_m_filter)
	function computeFilters(idx)
    
	Récupération du paramétrage des flux associés à l'objet dans les tables om_sig_map_flux
        et om_sig_map_flux
    function recupOmSigflux()
        requete dur om_sig_flux
        initialise le paramétrage de flux voir -> "champs flux"
    
	Initialisation des propriétés relatives aux fonds de carte externe,
        ajout des librairies associées si nécessaire
	function setParamsExternalBaseLayer()
        include de var_sig.inc
        initialise les "champs pour fonds de carte externes (OSM, Bing, Google)"
        
	Ecrit les propriétés de l'instance dans la page html pour JavaScript
	function prepareJS( )
        suite d'echo des variables 


Preparation et affichage du canevas ::

	Paramétrage des zones du canevas
	function setCanevas(zone, val) {
		this->affichageZones[zone]=val;
        
	Préparation du canevas html: pilote les autres fonctions prepareCanevas...
	function prepareCanevas( )
        initialisation du tableau this->affichageZones
    
	Préparation du canevas html: menu avec regroupement (au moins une valeur  à 1)
	function prepareCanevasMenu() {    

    Affichage du canevas MODIFIE JLB 
	function prepareCanevasTitre()
    function prepareCanevasEdit()
    function prepareCanevasTools() -> fonction vidée par jlb
    function prepareCanevasInfos() -> fonction vidée par jlb
    function prepareCanevasPrint() -> fonction enlevée par jlb
    function prepareCanevasLayers() -> reprend les 2 fonctions enlevées par jlb
    function prepareCanevasNavigation() -> modif jlb
    function prepareCanevasGetfeatures() -> modif jlb
    
	Calcul la géométrie validé dans l'interface -> appel par spg/map_compute_geom.php
	function getComputeGeom(seli, geojson)    
 
	Préparation du canevas html: pilote les autres fonctions prepareCanevas...
    -> appel par form_sig.php
	function prepareForm( min, max, validation, geojson)p_init()
    
voir core/obj et core/sql