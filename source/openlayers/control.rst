.. _control:



#######
control
#######

    
    
constructeur ::

var control = new OpenLayers.control.*(..)
        *   Attribution
            Button
            
            DragFeature
            DrawFeature
            ModifyFeature
            SelectFeature
            
            DragPan
            Pan
            Panel
            Panpanel
            PanZoom
            PanZoomBar
            
            ZoomBox
            ZoomIn
            ZoomOut
            ZoomPanel
            ZoomToMaxExtent
            
            EditingToolbar
            
            KeyboardDefaults
            MouseDefaults
            MousePosition
            MouseToolBar
            
            Navigation
            NavigationHistory
            NavToolBar
                        
            LayerSwitcher
            OverviewMap
            Permalink
            Scale
            ScaleLine
            

3 paramètres

Proprietes ::
    
    eventListeners          objet
    active                  boolean
    div                     DOMElement
    events                  array
    handlers                array
    map                     OpenLayers.Map

methods

    activate()              boolean
    deactivate              boolean
    initialize(options)
    moveTo(OpenLayers.Pixel)
    maximiseControl()
    destroy()
    