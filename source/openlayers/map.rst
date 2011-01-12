.. _map:

#########
objet map
#########


constructeur ::

    map = newOpnLayers.map(div, option)
    

div=id de l element  html
option= objet avec des proprietes non defaut

exemple ::

    var mercator = new OpenLayers.Projection("EPSG:900913");
    var world = new OpenLayers.Bounds(-180, -89, 180, 89).transform(geographic, mercator);
    var options = {
        projection: mercator,
        units: "m",
        maxExtent: world
    };
    var map = new OpenLayers.Map("map-id", options);



Proprietes ::

    events      OpenLayers.Events
    div         DOMElement
    layer       array(OpenLayers.Layer)
    tilesize    OpenLayers.size             taille des img tuiles
    projection
    units
    maxResolution  
    minResolution
    maxScale
    minScale
    maxExtent   OpenLayers.Bounds
    minExtend   OpenLayers.Bounds
    restrictedExtend OpenLayers.Bounds
    numZoomLevels
    theme
    displayProjection OpenLayers.Projction
    fallThrougt booleen
    eventListener objet

methodes ::

    *** set map
    setOptions({objet})
    setCenter()                             map.setCenter(arles, 12);
    *** zoom
    zoomTo(niveau de zoom)                                  
    zoomIn()
    zoomOut()
    zoomToExtent(OpenLayers.Bounds,closest=false)           
    zoomToMaxExtend(options)                map.zoomToMaxExtent();
    zoomToScale(echelle, closest=false)
    *** pan
    pan(dx,dy,opt)
    panTo()
    *** get
    getBy( , , )
    
    *** layer
    addLayer(OpenLayers.Layer)                              map.addLayer(imagery);
    addLayers([OpenLayers.Layer])
    removeLayer(OpenLayers.Layer, setNewBaseLayer=true)
    setBaseLayer(OpenLayers.Layer)
    
    
    *** control
    addControl(OpenLayers.Control, OpenLayers.Pixel)        map.addControl(new OpenLayers.Control.LayerSwitcher());



getCenter()
getZoom()
getExtent()
getScale()


