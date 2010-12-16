.. _framework:

##########
utilitaire
##########

=======================
utils.class
=======================

obj/utils.class.php


=======================
scripts complementaires
=======================

cette partie concerne la mise en oeuvre de script complementaire

Cela peut être :

un traitement : trt/
    exemple :
        opencourrier
            num_registre.php remise a 0 du registre
            archivage.php
un sous programme specifique (appelle d'un formulaire) : spg/
    exemple : openCourrier bible.php
                           js/script.js bible() 

un affichage scr/
    exemple : courrier.php : affichage d'un courrier
              recherchecourrier.php

le script commence par un appel a la bibliotheque utils.class.php et la creation d un objet $f

* obligatoire
10 require_once "../obj/utils.class.php";
11 $f = new utils(NULL, "courrier", _("recherche"), "ico_recherche.png", "recherche");

parametres 
    flag :
    right : droit géré en om_droit
    title : titre affiché
    icon  : icone affiché
    help  : aide affiché


* executer une requete dans un fichier sql
14 include ("../sql/".$f->phptype."/courrier_scr.inc");
15 $res=$f->db->query($sql_courrier);
16 $f->isDatabaseError($res);

* parcourir les enregistrements
17 while ($row=& $res->fetchRow(DB_FETCHMODE_ASSOC)){
18     echo $row['courrier'];
19 }

* ecrire dans la base

20  $sql = "INSERT INTO ... ";
21  $res2 = $f -> db -> query($sql);
22  $f->isDatabaseError($res2);

ou avec un tableau $valF

23 $obj = table
24 $valF[$obj]=$f-> db -> nextId(DB_PREFIXE.$obj);
25 $res1= $f-> db -> autoExecute(DB_PREFIXE.$obj,$valF,DB_AUTOQUERY_INSERT);
26 $f->isDatabaseError($res1);






 

    