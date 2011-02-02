.. _postgis_export:


######
export
######

===
klm
===

exemple::

    select cimetierelib, ST_asKML(transform(geom, 3385)) as geom from cimetiere

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
