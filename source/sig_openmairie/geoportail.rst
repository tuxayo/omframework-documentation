.. _geoportail:

##########
geoportail
##########

Le GéoPortail de l’IGN propose depuis avril 2008, en version bêta, l’accès à ses 
données par le biais d’une API JavaScript basée sur la bibliothèque de fonctions OpenLayers. 

La gestion des projections est réalisée par la version JavaScript de PROJ4, et les effets 
graphiques le sont par la bibliothèque script.aculo.us

Le fichier de l’API pèse 370ko compacté, ce qui est assez important  
L’utilisation  de l’API permet visualiser que 
le contenu classique du GéoPortail : cartes scannées et orthophotos :(base cadastre,
adresse, topo et ortho photo)

Il est aussi possible d’accéder aux données du GéoPortail par les services WMS/WMS-C et 
WFS, avec un système d’identification et de gestion des accès gratuit pour les collectivites. 

Affichage simple d’une fenêtre GéoPortail avec les paramètres par défaut ::

    <!DOCTYPE   html   PUBLIC   "-­‐//W3C//DTD   XHTML   1.0   Strict//EN"   "http://www.w3.org/TR/xhtml1/DTD/xhtml1-­‐ 
    strict.dtd">  
    <html  xmlns="http://www.w3.org/1999/xhtml">  
    Document sous licence CC-BY-SA 
    <head>  
        <title>API  Geoportail  -­‐  votre  carte  personnelle</title>  
        <script  src="http://api.ign.fr/api?v=1.0beta3&key=XXXXXXXX&instance=map"></script>  
      
        <script  type="text/javascript">  
      
            function  initGeoportalMap()  {  
                geoportalLoadmap("GeoportalMapDiv",  "normal",  "FXX");  
                if(map.allowedGeoportalLayers){  
                    map.addGeoportalLayers(map.allowedGeoportalLayers);  
                }  
            }  
        </script>  
    </head>  
    <body>  
        <div  id="GeoportalMapDiv"  style="width:400px;height:400px;"></div>  
    </body>  
    </html>
    
Personnalisation ne charger que la carte ::
    
    function initGeoportalMap() { 
        geoportalLoadmap("GeoportalMapDiv", "normal", "FXX"); 
        if(map.allowedGeoportalLayers){ 
            for (var i= 0; i<map.allowedGeoportalLayers.length; i++) { 
                var overloaded_options= null; 
                switch (map.allowedGeoportalLayers[i]) { 
                case 'GEOGRAPHICALGRIDSYSTEMS.MAPS': // cartes 
                     overloaded_options= { 
                        opacity: 1.0 
                     }; 
                     // On ajoute la couche 
                     map.addGeoportalLayer(map.allowedGeoportalLayers[i] 
                     ,overloaded_options); 
                     break; 
                case 'ORTHOIMAGERY.ORTHOPHOTOS': // ortho-photos 
                              // On ne fait rien 
                     break; 
                default : 
                     break; 
                } 
            } 
        } 
    } 

Modification de l’apparence des couches

Pour modificer l’apparence des couches affichées par l’API GéoPortail, il faut 
rechercher dans le DOM de la page les objets qui représentent les couches, pour ensuite 
pouvoir modifier leurs paramètres d’affichage. 

Exemple : Rendre invisible couche orthophotos 
http://www.geotests.net/apis/geoportail/ex1.html ::

    for(var i = 1; i < map.olMap.layers.length; i++){ 
          var layer= map.olMap.layers[i]; 
          if(layer.name == 'ORTHOIMAGERY.ORTHOPHOTOS') 
          { 
                  layer.setVisibility(false); 
          } 
    }
    
On passe en revue les couches (tableau layers de l’objet olMap) avec une boucle for. Si le 
layer en cours possède un paramètre nommé « ORTHOIMAGERY.ORTHOPHOTOS », on le 
rend invisible.

Ajout de données KML : On utilise simplement la fonction OpenLayers correspondant :: 

    function initGeoportalMap() { 
        geoportalLoadmap("GeoportalMapDiv", "normal", "FXX"); 
        if(map.allowedGeoportalLayers){ 
              map.addGeoportalLayers(map.allowedGeoportalLayers); 
        } 
        map.addLayer("KML", 
                { 
                   'terrasse.kml.name': 
                   { 
                       'fr':"Terrasse Carto", 
                   } 
                }, 
                "../google/carto.kml", 
                { 
                   minZoomLevel:10, 
                   maxZoomLevel:17 
                } 
        ); 
        map.setCenterAtLonLat(1.40220, 43.577531, 18); 
        map.setZoom(); 
    } 
                    
http://www.geotests.net/apis/geoportail/ex2.html 


