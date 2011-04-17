.. _map:

#########
objet map
#########


ce chapitre propose de décrire l'utilisation de l'objet map
d'openLayers dans om_sig.

Cet objet permet de définir

- le div ou la carte sera affiché (dans l'exemple :map-Id)

- les options de la carte et les controles affichés

constructeur ::

    map = newOpenLayers.map(div, option)
    
    
dans tab_sig_point_db.php ::

   map = new OpenLayers.Map('map-id',{projection: mercator,
			      units: "m",
			      maxResolution: "auto",
			      maxZoomLevel:18,
			      minZoomLevel: 10,
			      'maxExtent': etendue,
			      restrictedExtent: etendue,
			      controls: [
                    new OpenLayers.Control.ScaleLine({'bottomOutUnits':''}),
                    new OpenLayers.Control.PanZoomBar(),
                    new OpenLayers.Control.Navigation(),
                    new OpenLayers.Control.OverviewMap({maximized: true}),
                    new OpenLayers.Control.KeyboardDefaults(),
                    new OpenLayers.Control.MousePosition(),
                    new OpenLayers.Control.ZoomIn(),
                    new OpenLayers.Control.LayerSwitcher({'ascending':false})
			      ]}
			   );


========
methodes
========


Les methodes utilisées dans tab_sig_point_db.php sont les suivantes ::

    map.addLayer(s) pour ajouter une ou des couches
    map.setCenter(etendue, zoom); pour centrer sur l etendue défini et le zoom
    map.setOptions : pour changer les options
    map.addControl(controls[key]);  ajoute les contrôles du tableau de clé
    
Le zoom et l'etendu sont paramétrables :

- dans la table om_sig_point
- en url (pioritaire)

=============
Les controles
=============

Dans tab_sig_point_db.php, les controles sont initialisés :

- au niveau de l'objet map (voir plu haut)

- au niveau des layers


