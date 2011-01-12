.. _layer:



#####
layer
#####

couche base layer (couche qui s exclue): isBaseLayer= true
couche non base (ou overlay) : peuvent s afficher en meme temps

.. toctree::

    layer_wms.rst
    layer_osm.rst
    layer_vector.rst
    
    
constructeur ::

var layer = new OpenLayers.Layer.*
            * = WMS, WFS, GML, osm, Google

3 paramètres

Proprietes ::
    
    events                  OpenLayers.Events
    map                     OpenLayers.Map
    isBaseLayer             boolean
    displayInlayerSwitcher  boolean
    visibility              boolean
    attribution
    evenListerners          objet
    projection              OpenLayers.Projection|String
    units
    scales                  array
    resolutions             array
    maxExtent|minExtent     OpenLayers.Bounds
    maxResolution|minResolution
    maxScale|minScale
    numZoomLevels
    wrapDateLine            booleen
    transitionEffect

methods

    addOption(options)
    redraw()                        boolean
    removeMap(OpenLayers.Map)
    
    *** affecter les proprietes
    setName( nom)
    setTileSize()
    setVisibility(isVisible)        boolean
    setOpacity(opacity)
    setIsBaseLayer(isBase)
    display(isDisplayed)            
    
    *** recuperer ls proprietes
    getImageSize()                              boolean
    getResolution()                             float
    getExtent()                                 OpenLayers.Bounds
    getZoomForExtent(openLayers.bounds, )       integer
    getResolutionForZoom(zoom)                  float
    getZoomForResolution(resolution, )          integer
    getLonLatFromViewPartPx(openLayers.Pixel)   openLayers.LonLat
    getViewPortPxfromLonlat(openLayers.LonLat)  openLayers.Pixel