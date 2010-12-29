.. _formulaire:

###############
Les formulaires
###############

Les formulaires se construisent sur la base de la classe
formulairedyn.class.php d'openMairie

Cette classe fait appel a des sous programmes generiques pour certains
controles au travers de script js/formulairedyn.js



*************************************** 
Les methodes de formulairedyn.class.php
***************************************

La classe formulaire.class.php a les méthodes suivantes :

Les méthodes sur les controles du formulaire ::

    Hiddenstatic -> Champ non modifiable  Valeur récupéré par le formulaire
    Hiddenstaticdate -> date non modifiable Valeur récupéré par le formulaire
    statiq -> Valeur non modifiable
    date -> date modifiable  js/calendrier
    select -> Controle select
    selectdisabled -> Controle select non modifiable
    Text -> Controle text
    hidden -> Controle non visible avec valeur conservée
    password -> Controle password
    Textdisabled -> Controle text non modifiable
    comboG -> Appel à un programme de correspondance à une table
               Cas ou il y a une grosse table en correspondance
               spg/combo             
    ComboD -> Appel à un programme de correspondance à une table
               Cas ou il y a une grosse table en correspondance
               spg/combo
    Upload -> spg/upload et spg/voir.php
    textarea
    textarea_html
    localisation -> spg/localisation.php
    rvb -> spg/rvb.php
 
Les  méthodes de construction et d affichage ::


    afficher() affichage des champs (appelle par dbformdyn.class.php : methode formulaire
            -> afficherChampRegroupe() affichage des champs par regroupement / groupement
            -> afficherChamp() affichage de champ sans regroupe
    recupererPostvarsousform() et recuperePostVar():
                    recupèrent des variables apres validation
    enpied() presentation

Les méthodes assesseurs changent les valeurs des proprietes de l'objet form (formulaire) ::

    setType()
    setVal()
    setLib()
    setSelect()
    setTaille()
    setMax()
    setOnchange()
    setKeyup()
    setOnclick()
    setSelect()
    setGroupe()
        D premier champ du groupe
        G champ groupe
        F dernier champ du groupe
    setRegroupe()
        D premier champ du fieldset
        G champ dans le fieldset
        F dernier champ du fieldset

 
et enfin les méthodes de date ::

   dateAff($val)



==============================
Les sous programmes génériques
==============================



Les sous programmes génériques sont des sous programmes associés aux contrôles
du formulaire et appellés par eux par un script js dans js/formulairedyn.js 

Les sous programmes génériques sont stockés dans le répertoire /spg.

**spg/combo.php**


Ce programme est appellé par le contrôle comboD, comboG, comboD2, comboG2

  le paramétrage se fait dans les fichiers ::

       dyn/comboparametre.inc.php
       dyn/comboretour.inc.php
       dyn/comboaffichage.inc.php


**spg/localisation.php** et js/localisation.js

    
    ce programme est liée au contrôle formulaire "localisation"


**spg/voir.php** 

    Ce script est associé au contrôle "upload"
    
    Ce sous programme permet de visualiser un fichier téléchargé
    sur le serveur (pdf ou image)
    

**spg/upload.php**


        Ce script utilise la classe php/openmairie/upload.class.php (composant openMairie)

        Le paramétrage des extensions téléchargeables se fait dans le fichier autorise dans dyn/config.inc.php


**spg/rvb.php** et js/rvb.js


    Ce script est associé au contrôle "rvb" et permet l'accès à une palette de couleur
    pour récupérer un code couleur rvb



======================
le script scr/form.php
======================

form.php est le programme appellant d'un formulaire par rapport à un objet
métier(om_parametre) et un identifiant (2)

form.php affiche le formulaires et éventuellement les sous formulaires (soustab.php et sousform.php)

exemple ::

    form.php?obj=om_parametre&idx=2



=================================================================
Les nouvelles utilisations dans les objets metiers (openMairie 4)
=================================================================

openMairie4 apporte de nouvelles fonctions qu'il est utile d'implémenter dans
les objets métiers


**récuperer le type de la base** depuis l'objet db : $db->phptype ::


        if(file_exists ("../sql/".$db->phptype."/".$this->table.".form.inc"))/
			/include ("../sql/".$db->phptype."/".$this->table.".form.inc");/


**récuperer une erreur dans la base**

om4 ::

    database::isError($res); // ($res,true) = sans die


ce code remplace le code om3 (deprecated) ::

            //   if (DB :: isError($res))
            //            $this->erreur_db($res->getDebugInfo(),$res->getMessage(),'');
            //    else
            //    {
            //    if ($DEBUG == 1)
            //            echo "La requ&ecirc;te de mise &agrave; jour est effectu&eacute;e.<br>";
   