.. _layers:

############
objet layers
############


ce chapitre propose de décrire l'utilisation de l'objet layers
d'openLayers dans om_sig.


Dans tab_sig_point_db.php, il y a 2 types de layers :

- les fonds de cartes existants sur internet (base layers)
 
- les données issus de postgresql (overlays)


les fonds
=========

Il est proposé les fonds suivants :

osm ::

    var osm = new OpenLayers.Layer.OSM("OpenStreetMap");


sat : satelite google ::

    // acces à l API
    echo "<script src='http://maps.google.com/maps?file=api&amp;v=2&amp;
    key=ABQIAAAAjpkAC9ePGem0lIq5XcMiuhR_wWLPFku8Ix9i2SXYRVK3e45q1BQUd_beF8dtzKET_EteAjPdGDwqpQ'></script>";

    // layer
    var sat = new OpenLayers.Layer.Google(
      "Google Hybrid", {
         type: G_HYBRID_MAP,
         'sphericalMercator': true,
         'maxExtent': etendue,
         restrictedExtent: etendue,
         maxZoomLevel:30}
       );

bing ::

    // acces à l API
    echo "<script src='http://ecn.dev.virtualearth.net/mapcontrol/mapcontrol.ashx?v=6.2&amp;mkt=en-us'></script>";

    // layer
    var bing = new OpenLayers.Layer.VirtualEarth("Bing", { 
      sphericalMercator: true,
      'maxExtent': etendue,
          restrictedExtent: etendue,
      maxZoomLevel:30,
      animationEnabled: true
      }
    );


les datas
=========

Information de la carte :
layer_info ::

    layer_json = new OpenLayers.Layer.GML("data",tmp,{
         format:OpenLayers.Format.GeoJSON,
         formatOptions:{
             internalProjection:mercator,
             externalProjection:projection_externe
         },
          styleMap: new OpenLayers.StyleMap({
             "default": {externalGraphic: imgdata, graphicWidth:img_w, graphicHeight: img_h, graphicYOffset: -img_h},
             "select": {externalGraphic: img_hover,graphicWidth:  img_w_c, graphicHeight:  img_h_c, graphicYOffset: -img_h_c}
         })
     });

Cette couche fait appel à json_points.php

Il est possible de faire appel a un autre script (voirvar.point.inc)

La requête pgsql est paramétrée dans la table om_sig_point.

Requete ODP ::

    select astext(geom) as geom,
            (numero_voie||' '||libelle_voie) as titre,
            etablissement as description,
            odp as idx
            from odp order by geom,etablissement


json_points.php présente tous les enregistrements d'un même
point (même géom) sur un  seul popup

En effet, il est constitué un popup lorsque l on clique sur l objet
et donne la possibilité à un accès URL parametrée dans om_sig_point::

   ../scr/odp.php?idx=
   

Le point à modifier : couche vectors :
===================

Le chargement de la couche vectors se fait si dans om_sig_point,
la case maj est activé ::

      vectors = new OpenLayers.Layer.GML("vectors",tmp,{
		  format:OpenLayers.Format.WKT,
		  formatOptions:{
			  internalProjection:mercator,
			  externalProjection:projection_externe
		  },
	  styleMap: new OpenLayers.StyleMap({
	      "default": {strokeColor: "black",strokeWidth:3,strokeOpacity: 0.5,fillColor : "red", pointRadius : 5},
	      "select": {strokeColor: "black",strokeWidth:3,strokeOpacity: 0.5,fillColor : "green", pointRadius : 5}
	    })
      });



Le point est récupéré par le script wkt_point.php (appel a un script parametrable dans var_pointinc
et la carte est centrée sur ce point::

 il est possible de :
    
    - positionner manellement le point : onglet dessiner
    
    - déplacer le point : onglet déplacer
    
    - enregistrer le point  : selectionner le point, le programme
    form_sig_point.php est chargé en fenetre et permet de supprimer
    la géométrie (champ geometrique = null)  ou modifier cette géométrie.
    
    Les fonctions javascript et les controles sont activées suivant chaque état.
   