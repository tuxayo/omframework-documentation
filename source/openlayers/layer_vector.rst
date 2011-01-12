.. _layer_vectors:

#############
layer_vectors
#############

map3.html

var earthquakes = 	new OpenLayers.Layer.Vector("Earthquakes", {
				strategies: [new OpenLayers.Strategy.Fixed()],
				protocol: new OpenLayers.Protocol.HTTP({
				url: "7day-M2.5.xml",
				format: new OpenLayers.Format.GeoRSS()
				})


options
	strategie  : on peut declencher une strategie de modification / fixed : ne requete qu une fois 
	protocole de mise a jour client/serveur
	url : d'acces aux données
	format : ici geoRSS



