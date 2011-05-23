.. _postgis:

#######
postgis
#######

ce chapitre propose de décrire les possibilités d'utilisation de postgis dans openMairie.
Il n'est pas bien sûr exhaustif et se completera au fur à mesure de notre expérience
dans les applications openMairie.


principes
=========


PostGIS ajoute à postgresql ::

    - ses types de données
    - ses fonctions,  
    - deux tables utilitaires : geometry_colums et spatial_ref_sys.
        geometry_colums sert à indiquer au logiciel quels sont les champs contenant des types
        géographiques dans chacune des tables.
        spatial_ref_sys contient les paramètres des systèmes de projection  supportés
        et sert en interne au logiciel. 

Cf. : http://postgis.refractions.net/documentation/manual-1.3/ch06.html 

Le principe est donc de pouvoir : ::

    -  stocker des informations géographiques en tenant compte de ces 
       caractéristiques particulières (projection, dimensions, etc.), 
    -  réaliser des opérations de type SIG comme des mesures de distance, 
       dʼintersection, 
    -  créer de nouvelles géométries en les décrivant dans un format reconnu, ou 
       comme résultat dʼopérations sur des géométries existantes (arithmétiques), 
    -  retourner des informations géographiques (géométries et/ou attributaires) selon 
       un format précis, sur le réseau. 




sauvegarde
==========

sur postgis.fr ::

    $ pg-dump -Fc
    met la geometry au format wkb dans un fichier texte


strategie d utilisation de format
=================================

formats geometriques ::
    
    utiliser kml ou gml (plus standard) plutot que json (specifique js)
    pour moins de 100 geometries dans openLayers en export
    sinon utiliser mapserver avec :
            strategy : il n y a que la partie en fenetre qui est chargé
            ondraystop -> et chargements suivants en fonction du deplacement
    l export wkt permet d'utiliser plus facilement la géometrie sous openlayers


export klm
===========

exemple d utilisation de la fonction postgis ::

    select cimetierelib, ST_asKML(transform(geom, 3385)) as
        geom from cimetiere

script ::

    <?php
    include ("../obj/utils.class.php");
    $f = new utils ();
    $sql="select cimetierelib, ST_asKML(transform(geom, 3385)) as geom from cimetiere"; //900915
    $res = $f -> db -> query($sql);
    if (DB :: isError($res)){
       die($res->getMessage()."erreur ".$sql);
    }else{
        $kml = '<?xml version="1.0" encoding="UTF-8"?>' . "\n";
        $kml .= '<kml xmlns="http://earth.google.com/kml/2.2">' . "\n";
        $kml .= '<Document><name>cimetiere</name>';
            while ($row=& $res->fetchRow(DB_FETCHMODE_ASSOC)){
            $kml .= "<Placemark><name>libelle :
                ".$row['cimetierelib']."</name><description>".$row['cimetierelib']."</description>".$row['geom']."</Placemark>\n";
        }
    }
    $kml .= "</Document></kml>";
    header("Content-Type : text/xml; charset=utf8");
    echo $kml;
    ?>

export geojson
==============

http://dev.openlayers.org/docs/files/OpenLayers/Format/GeoJSON-js.html

Utilisation de la fonction postgis st_asgeojson

exemple ::

    select st_asgeojson(geom) from cimetiere

    "{"type":"Polygon","coordinates":
        [[
        [785186.978444590000436,155628.164220465143444],
        ...
        [785168.318216552375816,155556.067884865042288],
        [785186.978444590000436,155628.164220465143444]
        ]]
    }"


script php d export de  geogson avec la fonction postgis dans openCimetiere ::
    
    <?php
    include ("../obj/utils.class.php");
    $f = new utils ();
    $sql="select st_asgeojson(geom) as geom, cimetierelib, adresse1,cimetiere as n  from cimetiere"; 
    $res = $f -> db -> query($sql);
    if (DB :: isError($res)){
       die($res->getMessage()."erreur ".$sql);
    }else{
        $json = '{ "type": "FeatureCollection",'."\n";
        $json.= '  "features": ['."\n";
            while ($row=& $res->fetchRow(DB_FETCHMODE_ASSOC)){
                 if($row["geom"]!=''){
                    $json.= '{ "type": "Feature",'."\n";
                    $json.= '"geometry":'.$row["geom"].",\n";
                    $json.= '"properties": {'."\n";
                    $json.= '"cimetierelib": "'.$row['cimetierelib'].'"'.",\n";
                    $json.= '"adresse1": "'.$row['adresse1'].'"'."\n,";
                    $json.= '"n": "'.$row['n'].'"'."\n"; // derniere champ (sans virgule)
                    $json.= "}\n";
                    $json.= "},\n";
                 }
        }
    }
    $json=substr($json,0, strlen($json)-2);
    $json.= "]"."\n";
    $json.="}";
    echo $json;
    ?>
    
attention la fonction st_asgeojson n existe pas en version de postgis dans la la 1.3.5
(<1.5) 

script dans openodp qui n utilise pas la fonction postgis ::


export wkt
==========

Exemple de transfert donnees wkt ::

    <?php
    include ("../obj/utils.class.php");
    $f = new utils ();
    $sql="select astext(geom) as geom from cimetiere"; 
    $res = $f -> db -> query($sql);
    if (DB :: isError($res)){
        die($res->getMessage()."erreur ".$sql);
    }else{
        $wkt = 'GEOMETRYCOLLECTION(';
        while ($row=& $res->fetchRow(DB_FETCHMODE_ASSOC)){
             if($row["geom"]!='')
            $wkt.= $row["geom"].",";
        }
    }
    $wkt=substr($wkt,0, strlen($wkt)-1);
    $wkt.= ")";
    echo $wkt;
    ?>
    
import shp
==========

*** Recuperation fichier shp ::
-> 3 fichiers avec extension shp shx dbf
    shp2pgsql -D -I /home/utilisateur/cadastreArles/13004/PARCELLE_area.shp parcelle | psql cadastre 
    shp2pgsql -D -I /home/utilisateur/cadastreArles/13004/BATIMENT_area.shp batiment  | psql cadastre 
    par defaut column_geometry = -1
    
    shp2pgsql -s [srid] -I -D communes.shp | psql [base] 

exemple ::

    shp2pgsql -s 2154 -I -D DEPARTEMENT.SHP | psql postgis


fonctions postgis
=================

surface ::

    SELECT Area2d(geom) FROM cimetiere;

perimetre ::

    SELECT perimeter(geom) FROM cimetiere;
    
distance ::

    exemple demande_licence d'opendebitboisson

    selection des perimetre a moins de longueur_exclusion_metre (longueur du perimetre
    d'exclusion), $res0 est la valeur du point de l etablissement

    select distance(geom,'".$res0."') as distance,
    perimetre,libelle from perimetre
    where  distance(geom,'".$res0."')  < longueur_exclusion_metre
    order by perimetre";


ST_Touches : Les départements limitrophes du Tarn ::	 
  
    SELECT d.nom_dept 
    FROM departement as d, 
    (SELECT the_geom FROM departement WHERE 
    departement.code_dept = '81') as tarn 
    WHERE ST_Touches(d.the_geom, tarn.the_geom)
          
ST_buffers et ST_contains : Les départements à moins de 200km de la limite du Tarn ::

    SELECT DISTINCT d.code_dept, d.nom_dept 
    FROM departement as d, 
    (SELECT the_geom FROM departement WHERE 
    departement.code_dept = '81') as tarn, 
    ST_buffer(tarn.the_geom, 200000) as le_buffer 
    WHERE ST_contains(le_buffer, d.the_geom) 

ST_intersects : Les cours d'eau du Tarn ::

    SELECT DISTINCT ce.toponyme 
    FROM cours_eau as ce, 
    (SELECT the_geom FROM departement WHERE code_dept = '81') as tarn 
    WHERE ST_intersects(tarn.the_geom, ce.the_geom) 

Sous requete ST_contains : Les points de mesure hydrologiques du Tarn :: 

    SELECT DISTINCT mes.nom_usuel 
    FROM st_eausup_ag as mes, 
    (SELECT the_geom FROM departement WHERE code_dept = '81') as tarn 
    WHERE ST_Contains(tarn.the_geom, mes.the_geom) 

Double requête : Le nom des points de mesure a proximité de la Garonne :: 


    Création d’une table temporaire pour stocker un buffer de 100m autour de la Garonne : 
        create table bgaronne as 
        select st_buffer(the_geom, 100) as the_geom 
        from cours 
        where toponyme = 'La Garonne'
   
    Recherche des points de mesure dans ce polygone : 

        select st_eausup_ag.nom_usuel 
        from st_eausup_ag, bgaronne 
        where st_contains(bgaronne.the_geom, st_eausup_ag.the_geom) 


transform
=========

Il est proposé ici quelques utilisation de la fonction transform de postgis


recupérer le SRID d'une colone géométrique dan la table geometry_columns::

    select srid from geometry_columns where f_table_name='cimetiere';


Transformer du lamber sud en lambert 93 dans la meme table sur un champ different (geom_rf93)::
    
    SELECT addGeometryColumn( 'cimetiere', 'geom_rgf93', 2154, 'POLYGON', 2);

        CONSTRAINT enforce_dims_geom CHECK (st_ndims(geom) = 2),
        CONSTRAINT enforce_dims_geom_rgf93 CHECK (st_ndims(geom_rgf93) = 2),
        CONSTRAINT enforce_geotype_geom CHECK (geometrytype(geom) = 'POLYGON'::text OR geom IS NULL),
        CONSTRAINT enforce_geotype_geom_rgf93 CHECK (geometrytype(geom_rgf93) = 'POLYGON'::text OR geom_rgf93 IS NULL),
        CONSTRAINT enforce_srid_geom CHECK (st_srid(geom) = 27563),
        CONSTRAINT enforce_srid_geom_rgf93 CHECK (st_srid(geom_rgf93) = 2154)
    
    UPDATE cimetiere SET geom_rgf93 = transform(geom,2154);

    select astext(geom), astext(geom_rgf93) from cimetiere
    
        "POLYGON((785186.97844459 155628.164220465,
                785245.927801345 155601.446166684, ...
        "POLYGON((831753.180418834 6287843.04161093,
                  831811.956095086 6287815.89024952 ...


Transformer un champ geom en mercator et envoi en fichier kml  ::

    select cimetierelib, ST_asKML(transform(geom, 3385)) as geom from cimetiere


Transformer un point en geographic ::

    select astext(transform(setsrid(geometryfromtext
        ('POINT(4.632438 43.684858)'),4326),27563));
        
                POINT(785074.087673277 156458.572267362)
        
