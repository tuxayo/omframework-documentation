.. _utilitaire:

##########
utilitaire
##########


Les méthodes spécifiques a l application sont dans obj/utils.class.php qui héritent de la class om_application.class.php d 'openmairie
Vous pouvez surcharger les classes dans utils.class.php

Ces classes contiennent les méthodes pour développer les scripts complémentaires de votre application.


***********************
scripts complementaires
***********************

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


========
tutorial
========

le script commence par un appel a la bibliotheque utils.class.php et la creation d un objet $f

**obligatoire** ::

    require_once "../obj/utils.class.php";
    $f = new utils(NULL,
                "courrier",
                _("recherche"),
                "ico_recherche.png",
                "recherche");

*parametres* 

    flag : si flag= Null affichage complete

                    nonhtml : pas d affichage

                    htmlonly : tout les elements externes html avec body vide

    right : droit géré en om_droit - vide ne verifie pas

    title : titre affiché

    icon  : icone affiché

    help  : aide affiché

*Verification*

- si l utilisateur est authentifié

- si l utilisateur a le droit

**Methode complementaire si right est vide** ::

    isAccredited() // a le droit ou pas
    isAuthentified // si non authentifié, il est rejeté
    
    $f->setRight($obj); // affecte un drait d acces
    $f->isAuthorized(); //verification que l utilisateur accede

    // Affectation des variables en dehors du constructeur 
    $f->setTitle($ent);
    $f->setIcon($ico);
    $f->setHelp($obj);
    $f->setFlag(NULL);
    
    // affichage 
    $f->display();    

**executer une requete dans un fichier sql** ::
    
    include ("../sql/".$f->phptype."/courrier_scr.inc");
    
    $res=$f->db->query($sql_courrier);
    
    $f->isDatabaseError($res);

**parcourir les enregistrements** ::
    
    while ($row=& $res->fetchRow(DB_FETCHMODE_ASSOC)){
    
         echo $row['courrier'];
    
    }

**ecrire dans la base** ::

    $sql = "INSERT INTO ... ";

    $res2 = $f -> db -> query($sql);

    $f->isDatabaseError($res2);

*ou avec un tableau $valF* ::

    $obj = table
    
    $valF[$obj]=$f-> db -> nextId(DB_PREFIXE.$obj);
    
    $res1= $f-> db -> autoExecute(DB_PREFIXE.$obj,$valF,DB_AUTOQUERY_INSERT);
    
    $f->isDatabaseError($res1);


**Description du role de la page** ::

    $description = _("Cette page vous permet de .. ");
    
    $f->displayDescription($description);

**message d erreur**

    $class : classe css qui s'affiche sur l'element
    
        "error" : message erreur
    
        "valid" : message de validation

    
*code* ::
    
    $message = _("Mot de passe actuel incorrect");
    $f->displayMessage($class, $message);

**fieldset** ::

    echo "<fieldset class=\"cadre ui-corner-all ui-widget-content\">\n";
    
    echo "\t<legend class=\"ui-corner-all ui-widget-content ui-state-active\">";
    
    echo _("Courrier")."</legend>";
        ...
    echo "</fieldset

*ouvert* ::

    echo "<fieldset class= ... collapsible\">\n";

*ferme* ::

    echo "<fieldset ... startClosed\">\n";


**appel a des scripts js complementaires** ::

    $f->addHTMLHeadJs(array("../js/formulairedyn.js", "../js/onglet.js"));

**gestion des accents**

    de base ne pas mettre d accent dans le code (utf8 au lieu de latin1-iso8859-1)
    
    mettre les accents dans la traduction

**path upload de fichier** ::
  
  $path=$f->getPathFolderTrs()




    