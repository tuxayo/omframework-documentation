.. _more:

#######################
Pour aller plus loin...
#######################


.. contents::

.. _utilitaire:

****************************************
Les scripts specifiques de l'application
****************************************



============
Introduction
============

Les méthodes spécifiques à l'application sont dans obj/utils.class.php
qui héritent de la class om_application.class.php d 'openmairie

Vous pouvez surcharger les classes d'om_application.class.php dans utils.class.php

Exemple : surcharge de la méthode login() pour conserver le service d'un utilisateur
en variable session dans openCourrier.

Ces classes contiennent les méthodes utilisées par le framework mais
qui peuvent vous aider à développer les scripts complémentaires de votre application.

Les scripts complémentaires sont mis en répertoire app / et peuvent être créer pour :

- faire un traitement (remise à 0 d'un registre, archivage, export ....)

- faire un sous programme spécifique appellé par un formulaire (bible.php dans openCourrier)

- faire une recherche avec un affichage particulier

Les scripts javascripts sont mis dans le fichier app/js/script.js

Les images spécifiques sont stockées dans app/img


=================================
Réaliser un script complementaire
=================================

Il est proposé ici de vous montrer comment réaliser ce script complémentaire

Le script commence obligatoirement par un appel à la bibliothèque utils.class.php et la creation d un objet $f::

    require_once "../obj/utils.class.php";
    $f = new utils(NULL,
                "courrier",
                _("recherche"),
                "ico_recherche.png",
                "recherche");

Les parametres de l'objet sont les suivants :

    flag : si flag= Null affichage complete

                    nonhtml : pas d affichage

                    htmlonly : tout les elements externes html avec body vide

    right : droit géré en om_droit - vide ne verifie pas

    title : titre affiché

    icon  : icone affiché

    help  : aide affiché



utils.class.php fait la Verification si l utilisateur est authentifié et si l utilisateur a le droit
(util.class surcharge core/om_application.class.php qui contient les scripts de base du framework)


Si le paramétre "right" est vide vous pouvez faire appel aux méthodes suivantes ::

    isAccredited() // a le droit ou pas
    isAuthentified // si non authentifié, il est rejeté
    
    $f->setRight($obj); // affecte un droit d acces
    $f->isAuthorized(); //verification que l utilisateur accéde

    // Affectation des variables en dehors du constructeur 
    $f->setTitle($ent);
    $f->setIcon($ico);
    $f->setHelp($obj);
    $f->setFlag(NULL);
    
    // affichage 
    $f->display();    

Pour **executer une requête dans un fichier sql** vous devez stocker
votre requête dans le répertoire sql/type_de_sgbd/nom_de_requete.inc
afin de préserver la portabilité de vos travaux sur d'autres sgbd::
    
    // appel au fichier requête
    include ("../sql/".$f->phptype."/courrier_scr.inc");
    
    // lancement de la requete sql_courrier et test erreur
    $res=$f->db->query($sql_courrier);
    $f->isDatabaseError($res);

Pour **parcourir les enregistrements** vous utilisez les méthodes dbpear suivantes::
    
    // du debut à la fin de la requête
    while ($row=& $res->fetchRow(DB_FETCHMODE_ASSOC)){
        // j'affiche le champ courrier
         echo $row['courrier'];
    
    }

Pour **ecrire dans la base** vous pouvez utiliser les méthodes insert ou update
mais vous pouvez utilisez la méthode autoexecute spécifique à db pear:

*requête sql* ::

    $sql = "INSERT INTO ... ";

    $res2 = $f -> db -> query($sql);

    $f->isDatabaseError($res2);

*ou avec un tableau $valF* ::

    $obj = table
    
    $valF[$obj]=$f-> db -> nextId(DB_PREFIXE.$obj);
    
    $res1= $f-> db -> autoExecute(DB_PREFIXE.$obj,$valF,DB_AUTOQUERY_INSERT);
    
    $f->isDatabaseError($res1);


Vous pouvez faire une **Description du role de la page** de la manière suivante ::

    $description = _("Cette page vous permet de .. ");
    
    $f->displayDescription($description);

Un **message d erreur** s'affiche suivant :

    $class : qui est la classe css qui s'affiche sur l'element et qui peut être
    
        "error" : pour le message erreur
    
        "valid" : pour le message de validation

    
le *code* est le suivant ::
    
    $message = _("Mot de passe actuel incorrect");
    $f->displayMessage($class, $message);

Pour afficher  un **fieldset**, le code est le suivant ::

    echo "<fieldset class=\"cadre ui-corner-all ui-widget-content\">\n";
    
    echo "\t<legend class=\"ui-corner-all ui-widget-content ui-state-active\">";
    
    echo _("Courrier")."</legend>";
        ...
    echo "</fieldset>


il peut être par défaut *ouvert* ::

    echo "<fieldset class= ... collapsible\">\n";

ou il peut être *fermé* ::

    echo "<fieldset ... startClosed\">\n";


Vous pouvez faire **appel a des scripts js complementaires** en utilisant la méthode ::

    $f->addHTMLHeadJs(array("../js/formulairedyn.js", "../js/onglet.js"));

Pour la **gestion des accents**, il est conseillé de ne pas mettre d accent dans
le code (utf8 au lieu de latin1-iso8859-1) et de mettre les accents dans la traduction

Pour définir le chemin par défaut pour l' ** upload de fichier**, il faut utiliser la méthode ::
  
  $path=$f->getPathFolderTrs()

=======
Exemple
=======

Il est proposé de prendre l'exemple du traitement de la remise du registre
a 0 dans openCourrier ::

    
    
    // ENTETE NORMALISEE
    
    /**
     * Cette page permet de remettre a 0 le registre
     *
     * @package openmairie_exemple
     * @version SVN : $Id: xxxx.php 311 2010-12-06 11:43:36 xxxxx $
     */
    
    
    // CREATION DE L' OBJET $f
    
    require_once "../obj/utils.class.php";
    $f = new utils(NULL, "traitement", _("remise a 0 du registre"), "ico_registre.png", "recherche");

    
    
    // get
    if (isset ($_GET['validation'])){
       $validation=$_GET['validation'];
    }else{
       $validation=0;
    }

    
    /**
     * Description de la page
     */
    
    $description = _("Cette page vous permet de remettre a 0 le numero de registre ".
                     "Ce traitement est a faire en debut d annee.");
    $f->displayDescription($description);


    // TEST VALIDATION
    // SI = 0 affichage du numero de registre
    // SI = 1 mise à 0 du registre et affichage du résultat
    
    if($validation==0){
        $validation=1;
        
        // REQUETE DU REGISTRE
        
        $sql= "select id from registre_seq" ;
        $res1=$f->db->getOne($sql);
        $f->isDatabaseError($res1);
        
        // AFFICHAGE DANS UN FIELDSET
        
        echo "<fieldset class=\"cadre ui-corner-all ui-widget-content\">\n";
        echo "\t<legend class=\"ui-corner-all ui-widget-content ui-state-active\">";
        echo _("Registre ")."</legend>";
        if ($res1!=0){
            echo "<br>"._("le dernier no du registre est")." : &nbsp;&nbsp;".$res1."&nbsp;&nbsp;";
        }else{
            echo "<br>"._("vous avez deja fait une remise a 0")."<br>";
        }
        echo "<form method=\"POST\" action=\"num_registre.php?validation=".
        $validation."\" name=f1>";
        echo "</fieldset>";
        
        // BOUTON DE VALIDATION
        echo "\t<div class=\"formControls\">";
        echo "<input type='submit' value='"._("remise a 0 du registre").
              "&nbsp;' >";
        echo "</div>";
        echo "</form>";
    
    }else { // validation=1
        
        // VALORISATION DE $valF
        $valF=array();
        $valF['id']=0;
        
        // REQUETE MISE A JOUR avec autoExecute
        $res2= $f->db->autoExecute("registre_seq",$valF,DB_AUTOQUERY_UPDATE);
        $f->isDatabaseError($res2);
    
        // AFFICHAGE DU RESULTAT AVEC UN FIELDSET
        echo "<fieldset class=\"cadre ui-corner-all ui-widget-content\">\n";
        echo "\t<legend class=\"ui-corner-all ui-widget-content ui-state-active\">";
        echo _("Registre ")."</legend>";
        echo "<center><b>"._("remise a 0 du registre reussie")."</b></center>";
        echo "</fieldset>";
    
    }//validation


Notes

_("Registre ") : _("texte") permet l'utilisation de poedit pour la traduction de texte


class=\"cadre ui-corner-all ui-widget-content\" : suivant css de jquery
