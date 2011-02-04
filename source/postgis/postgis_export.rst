.. _postgis_export:


######
export
######

===
klm
===

exemple::

    select cimetierelib, ST_asKML(transform(geom, 3385)) as
        geom from cimetiere

script ::

    <?php
    $_SESSION['profil'] = 5;
    $_SESSION['nom'] = "admin";
    $_SESSION['login'] = "admin";
    $_SESSION['coll'] = 2;
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



=======
geojson
=======

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

fabrication d un geogson : openCimetiere ::
    
    <?php
    $_SESSION['profil'] = 5;
    $_SESSION['nom'] = "admin";
    $_SESSION['login'] = "admin";
    $_SESSION['coll'] = 2;
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

===
wkt
===

Exemple de transfert donnees wkt ::

    <?php
    $_SESSION['profil'] = 5;
    $_SESSION['nom'] = "admin";
    $_SESSION['login'] = "admin";
    $_SESSION['coll'] = 2;
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

