.. _js:

##############
Le java script
##############

les fonctions sont dans js/sig.js

Nouvelles fonctions ::

    
    affectation d'un mode d'action :
        Géolocalisation, Information,
        Navigation, Edition, Formulaire,
        Mesure surface, Mesure distance
        function map_set_mode_action(mode)
    
    function popupIt pour utilisation dans sig
        function map_popupIt(objsf, link, width, height, callback, callbackParams) {

    implémentation simplifiée de l'utilisation de popupIt dans la fonction SIG
    function map_popup(url
    
    vide les div du div map-getfeatures
    function map_clear_getfeatures()
    
    ouverture du div map-getfeatures 
    function map_close_getfeatures()
    
    affichage dans la zone info
    function map_affiche_info(message)
    
    affichage dans la zone titre
    function map_affiche_titre()
    
    Traitement du select d'un élement de la couche des marqueurs
    function map_on_select_feature_marker(event)
    
    Traitement du unselect d'un élement de la couche des marqueurs
    function map_on_unselect_feature_marker(event)
    
    Traitement du select d'un élement de la couche des datas
    function map_on_select_feature_data(event)
    
    Traitement du unselect d'un élement de la couche des datas
    function map_on_unselect_feature_data(event)
    
    récupère la valeur d'un attribut (traitement GetFeatureInfo)
    function traiteGetFeatureInfoRecAttribut(attributes,name)
    
    récupère la valeur d'un attribut et formate la restitution (traitement GetFeatureInfo)
    function traiteGetFeatureInfoRecAttributFormat(attributes,name,title, formatage)
    
    traitement pour chaque GetFeatureInfo
    function traiteGetFeatureInfo(i,rText,res)
    
    Traitement du clic sur la carte
    function map_clic(e) 
    
    copie les données geoson correspondant à l'idx sélectionné dans les
    couches d'édition correspondantes
    function map_copyDataToEditLayers()
    
    Traitement clic Edit Draw Point
    function map_clicEditDrawPoint()
    
    Traitement clic Edit Draw Line
    function map_clicEditDrawLine()
    
    Traitement clic Edit Draw Polygon
    function map_clicEditDrawPolygon()
    
    //traitement du onChange sur le nombre de côté du polygone régulier
    function map_EditDrawRegularChange()
    
    Traitement clic Edit Draw Polygon
    function map_clicEditDrawRegular()
     
    Traitement clic Edit modify
    function map_clicEditDrawModify()
    
    Traitement clic Edit select
    function map_clicEditSelect()
    
    Traitement clic Edit navigate
    function map_clicEditNavigate()
    
    Traitement clic Edit Erase
    function map_clicEditErase()
    
    test si une valeur est un entier
    function map_is_int(value)
    
    désactive tous les controles d'édition sauf sauf
    function map_deactivate_all_edit_control(sauf)
    
    affiche/cache les éléments de la barre d'outils d'édition
    function map_RefreshEditBar()
    
     calcule et valide les champs géométriques dont les composants sont sélectionnés
    function map_computeGeom(sel, enreg)
    
    traitement du clic sur le bouton de récupération du panier
    function map_clicEditGetCart()
    
    Selection d'un panier
    function map_select_cart(n)
    
    Selection d'un champ d'édition
    function map_select_edit_champ(n)
    
    Traitement géolocalisation
    function map_clicGeolocate()
    
    traitement bouton Edit
    function map_clicEdit()
    
    traitement bouton Edit Close
    function map_clicEditClose()
    
    Traitement de l'application des SLD pour les données geojson
    function map_successGetSld_geojson_datas(req)
    
    Chargement des données aux formats geojson
    function map_load_geojson_datas(bRefreshFilters)
    
    Traitement de l'application des SLD pour les marqueurs geojson
    function map_successGetSld_geojson_markers(req)
    
    Chargement des marqueurs aux formats geojson
    function map_load_geojson_markers()
    
    fonction d'identification des tuiles pour les flux de type SMT
    function map_flux_SMT(bounds)
    
    Initialisation des flux (om_sig_map_flux)
    function map_load_flux(i)
    
    Chargement des flux de type base
    function map_load_bases_layers()
        fonction qui charge les fonds osm, bing, sat ou un numéro de flux
    
    vide une des listes du selecteur (div map-layers)
    (dest: baselayers, overlays, datas, markers)
    function map_empty_layer_list(dest) 
    
    Chargement des flux de type overlays
    function map_load_overlays()
    
    Chargement des flux de type cart
    function map_load_carts()
    
    Affichage de la couche de base sélectionnée
    function map_display_base_layer()
    
    Ajoute du control openlayers de géolocalisation
    function map_addGeolocateControl()
    
    Ajoute le controle openLayers de sélection
    function map_add_SelectControl()
    
    Traitement des controles de mesure
    function map_handleMeasurements(event)
    
    Ajoute des controles de mesure
    function map_add_MeasureControls()
    
    Ajoute les contrôles openLayers à la carte
    function map_add_controls()
    
    Initialisation de la carte
    function map_init()
    
    
affichage en onglet  (jlb) ::

    affiche_aide
    affiche_layers
    affiche_tools
    affiche_baselayers
    affiche_getfeatures
    
A voir ::

    pb d affichage info si utilisation boite a outil : navigation ou geoloc ou
    mesure distance ou mesure aire.
    

    
