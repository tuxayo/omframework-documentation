.. _formulaire:

###############
Les formulaires
###############

La gestion des formulaires se base sur deux classes :
    - core/om_formulaire.class.php
    - core/om_dbform.class.php

om_formulaire.class.php permet la gestion de l'affichage, om_dbform.class.php permet la gestion de la logique interne.

La classe om_formulaire.class.php fait appel à des sous programmes génériques pour certains
controles au travers de script js/script.js

Les classes métier étendent de db_form.class.php, c'est donc dans ces classes que les formulaires seront préparés via l'appel aux méthodes présentes dans la classe om_formulaire.

Pour chaque formulaire de classe créer, le script sql/DBTYPE/[classe].form.inc.php correspondant est appelé. Il permet de stocker le detail des requètes necessaires à l'affichage du contenu du formulaire.

Les formulaires sont construits lors de l'appel aux scripts scr/form.php et scr/sousform.php. En fonction des paramètres passés à l'url, un formulaire de la classe métier correspondante sera créé.

********************************
Scripts form.php et sousform.php
********************************

L'appel à ce script peut se faire depuis un :ref:`tableau d'éléments<affichage>` ou depuis la :ref:`consultation d'un élément<action-consultation>`.

Liste des paramètres passés à l'url :
    - obj : nom de la classe pour laquelle on souhaite afficher le formulaire
    - action : type d'action (ajout, modification, suppression, consultation)
    - idx : identifiant (dans la base de données) de l'élément sur lequel on souhaite effectuer l'action (sauf en ajout)
    - retour : deux valeurs possible tab ou form selon l'origine de l'action

Les autres paramètres passés permettent de conserver la configuration du tableau d'origine.

*************************************** 
Les methodes de om_formulaire.class.php
***************************************

.. class:: formulaire($unused = NULL, $validation, $maj, $champs = array(), $val = array(), $max = array())

   La classe om_formulaire.class.php a les méthodes suivantes :

Liste des types de champs :

    .. method:: formulaire.text()

       controle text (format standard)

    .. method:: formulaire.hidden()

       controle non visible avec valeur conservée

    .. method:: formulaire.password()

       controle password

    .. method:: formulaire.textdisabled()

       controle text non modifiable

    .. method:: formulaire.textreadonly()

       contrôle text non modifiable

    .. method:: formulaire.hiddenstatic()

       champ non modifiable  Valeur récupéré par le formulaire

    .. method:: formulaire.hiddenstaticnum()

       champ numerique non modifiable et valeur récupérer

    .. method:: formulaire.statiq()

       Valeur affichée et non modifiable

    .. method:: formulaire.affichepdf()

       récupére un nom d'objet (un scan pdf)


    .. method:: formulaire.checkbox()

       controle case à cocher valeurs possibles : ``True`` ou ``False``

    .. method:: formulaire.checkboxstatic()

       affiche Oui/Non, non modifiable (mode consultation)

    .. method:: formulaire.checkboxnum()

       cochée = 1 , non cochée = 0


    .. method:: formulaire.http()

       lien http avec target = _blank (affichage dans une autre fenêtre)

    .. method:: formulaire.httpclick()

       lien avec affichage dans la même fenêtre.


    .. method:: formulaire.date()

       date modifiable avec affichage de calendrier jquery

    .. method:: formulaire.date2()

       date modifiable avec affichage de calendrier jquery pour les sous formulaire

    .. method:: formulaire.hiddenstaticdate()

       date non modifiable Valeur récupéré par le formulaire

    .. method:: formulaire.datestatic()

       affiche la date formatée, non modifiable (mode consultation)

    .. method:: formulaire.textarea()

       affichage d un textarea

    .. method:: formulaire.textareamulti()

       textarea qui récupére plusieurs valeurs d'un select

    .. method:: formulaire.textareahiddenstatic()

       affichage non modifiable d'un textarea et recupération de la valeur

    .. method:: formulaire.pagehtml()

       affichage d'un textarea et tranforme les retour charriot en <br>

    .. method:: formulaire.select()

       controle select

    .. method:: formulaire.selectdisabled()

       controle select non modifiable

    .. method:: formulaire.selectstatic()

       affiche la valeur de la table liée, non modifiable (mode consultation)

    .. method:: formulaire.selecthiddenstatic()

       affiche la valeur de la table liée, non modifiable ainsi que la valeur dans un champ hidden

    .. method:: formulaire.comboG()

       permet d'effectuer une correlation entre un groupe de champ et un identifiant dans les formulaires

    .. method:: formulaire.comboG2()

       permet d'effectuer une correlation entre un groupe de champ et un identifiant dans les sous formulaires

    .. method:: formulaire.comboD()

       permet d'effectuer une correlation entre un groupe de champ et un identifiant dans les formulaires

    .. method:: formulaire.comboD2()

       permet d'effectuer une correlation entre un groupe de champ et un identifiant dans les sous formulaires

    .. method:: formulaire.upload()

       fait appel à spg/upload.php pour télécharger un fichier

    .. method:: formulaire.upload2()

       fait appel à spg/upload.php pour télécharger un fichier dans un sous formulaire

    .. method:: formulaire.voir()

       fait appel à spg/voir.php pour visualiser un fichier

    .. method:: formulaire.voir2()

       fait appel à spg/voir.php pour visualiser un fichier depuis un sous formulaire

    .. method:: formulaire.localisation()

       fait appel à spg/localisation.php

    .. method:: formulaire.localisation2()

       fait appel à spg/localisation.php

    .. method:: formulaire.rvb()

       fait appel à spg/rvb.php pour affichage de la palette couleur

    .. method:: formulaire.rvb2()

       fait appel à spg/rvb.php pour affichage de la palette couleur

    .. method:: formulaire.geom()

       ouvre une fenetre tab_sig.php pour visualiser ou saisir une geometrie (selon l'action) la carte est définie en setSelect

Les contrôle comboG, comboD, date, upload, voir et localisation sont à mettre dans
les formulaires (retour de l'affichage dans le formulaire f1)
Les contrôle comboG2, comboD2, date2, upload2, voir2 et localisation sont à mettre dans
les sous formulaires (retour de l'affichage dans le formulaire f2)



Les  méthodes de construction et d'affichage
--------------------------------------------

Le formulaire est constitué de div, fieldset et de champs les méthodes suivante permettent une mise en page structuré.

    .. method:: formulaire.entete() / enpied()

       ouverture du conteneur du formulaire.

    .. method:: formulaire.afficher()

       affichage des champs, appelle les méthodes suivante :

    .. method:: formulaire.debutFieldset()

       ouverture de fieldset.

    .. method:: formulaire.finFieldset()

       fermeture de fieldset

    .. method:: formulaire.debutBloc()

      ouverture de div.

    .. method:: formulaire.finBloc()

      fermeture de div.

    .. method:: formulaire.afficherChamp()

       affichage de champ.

    .. method:: formulaire.recuperePostVar()

       recupèrent des variables apres validation d'un formulaire

    .. method:: formulaire.recupererPostvarsousform()

       recupèrent des variables apres validation d'un sous formulaire

Depuis la version 4.3.0 :

    .. method:: formulaire.transformGroupAndRegroupeToLayout()

       permet de garder la compatibilité des méthodes setGroupe() et setRegroupe() avec setLayout() (obsolètes depuis la version 4.3.0).

.. _méthodes-assesseurs:

Les méthodes assesseurs changent les valeurs des attributs de l'objet formulaire
--------------------------------------------------------------------------------

Ces méthode sont appelées depuis les classes métier.

    .. method:: formulaire.setType()

       type de champ.

    .. method:: formulaire.setVal()

       valeur du champ

    .. method:: formulaire.setLib()

       libellé du champ

    .. method:: formulaire.setSelect()

       permet de remplir les champs select avec la table liée

    .. method:: formulaire.setTaille()

       taille du champ

    .. method:: formulaire.setMax()

       nombre de caractères maximum acceptés

    .. method:: formulaire.setOnchange()

       permet de définir des actions sur l'événement

    .. method:: formulaire.setKeyup()

       permet de définir des actions sur l'événement

    .. method:: formulaire.setOnclick()

       permet de définir des actions sur l'événement

    .. method:: formulaire.setvalF()

       permet de traiter les données avant insert/update dans la base de données

    .. method:: formulaire.setGroupe()

       (obsolète depuis 4.3.0)

    .. method:: formulaire.setRegroupe()

       (obsolète depuis 4.3.0)

    .. method:: formulaire.setBloc($champ, $contenu, $libelle = '', $style = '')

       permet d'ouvrir/fermer ($contenu=D/F) une balise div sur un champ ($champ), avec un libellé ($libelle) et un attribut class ($style).

    .. method:: formulaire.setFieldset($champ, $contenu, $libelle = '', $style = '')

       permet d'ouvrir/fermer ($contenu=D/F) un  fieldset sur un champ ($champ), avec une legende ($libelle) et un attribut class ($style).





Mise en forme des formulaires
-----------------------------

.. _setLayout:

    - setLayout(), méthode de mise en page de la classe om_db_form.class.php, permet de gérer la hierarchie d'ouverture et fermeture des balises div et fieldset avec les méthodes :
        - setBloc($champ, $contenu, $libelle = '', $style = '') \: permet d'ouvrir/fermer ($contenu=D/F) une balise div sur un champ ($champ), avec un libellé ($libelle) et un attribut class ($style).
            - une liste de classes css pour fieldset est disponible :
                - group : permet une mise en ligne des champs contenu dans le div
                - col_1 à col_12 : permet une mise en page simplifiée, par exemple : "col_1" permet de définir une taille dynamique de 1/12ème de la page , col_6 correspond à 6/12 soit 50% de l'espace disponible.
        - setFieldset($champ, $contenu, $libelle = '', $style = '') \: permet d'ouvrir/fermer ($contenu=D/F) un  fieldset sur un champ ($champ), avec une legende ($libelle) et un attribut class ($style).
            - une liste de classes css pour fieldset est disponible :
                - collapsible : ajoute un bouton sur la legende (jQuery) afin de refermer le fieldset.
                - startClosed : idem à la difference que le fieldset est fermé au chargement de la page.
        - exemple d'implémentation de la méthode setLayout() afin d'obtenir le même affichage sans utiliser les méthodes setGroupe() et setRegroupe() : ::

            function setLayout(&$form, $maj) {
                //Ouverture d'un fieldset
                $form->setFieldset('om_collectivite','D',_('om_collectivite'), "collapsible");
                    //Ouverture d'un div les champs compris entre "om_collectivite" et "actif"
                    //la classe group peremet d'afficher les champs en ligne
                    $form->setBloc('om_collectivite','D',"","group");
                    //Fermeture du groupe
                    $form->setBloc('actif','F');
                //Fermeture du fieldset
                $form->setFieldset('actif','F','');

                $form->setFieldset('orientation', 'D', _("Parametres generaux du document"), "startClosed");
                    $form->setBloc('orientation','D',"","group");
                    $form->setBloc('format','F');

                    $form->setBloc('footerfont','D',"","group");
                    $form->setBloc('footertaille','F');

                    $form->setBloc('logo','D',"","group");
                    $form->setBloc('logotop','F');
                $form->setFieldset('logotop','F','');

                $form->setFieldset('titreleft','D',_("Parametres du titre du document"), "startClosed");
                    $form->setBloc('titreleft','D',"","group");
                    $form->setBloc('titrehauteur','F');

                    $form->setBloc('titrefont','D',"","group");
                    $form->setBloc('titrealign','F');
                $form->setFieldset('titrealign','F','');

                $form->setFieldset('corpsleft','D',_("Parametres du corps du document"), "startClosed");
                    $form->setBloc('corpsleft','D',"","group");
                    $form->setBloc('corpshauteur','F');

                    $form->setBloc('corpsfont','D',"","group");
                    $form->setBloc('corpsalign','F');
                $form->setFieldset('corpsalign','F','');

                $form->setFieldset('om_sousetat','D', _("Sous etat(s) : selection"), "startClosed");
                    $form->setBloc('om_sousetat','D',"","group");
                    $form->setBloc('sousetat','F');
                $form->setFieldset('sousetat','F', '');

                $form->setFieldset('se_font','D', _("Sous etat(s) : police / marges / couleur"), "startClosed");
                    $form->setBloc('se_font','D',"","group");
                    $form->setBloc('se_couleurtexte','F');
                $form->setFieldset('se_couleurtexte','F','');
            }

            .. note ::
               test


==============================
Les sous programmes génériques
==============================



Les sous programmes génériques sont des sous programmes associés aux contrôles
du formulaire et appellés par eux par un script js dans js/formulairedyn.js 

Les sous programmes génériques sont stockés dans le répertoire /spg.

**spg/combo.php**


Ce programme est appellé par le contrôle comboD, comboG, comboD2, comboG2, le paramétrage se fait dans les fichiers :

    - dyn/comboparametre.inc.php
    - dyn/comboretour.inc.php
    - dyn/comboaffichage.inc.php


**spg/localisation.php** et js/localisation.js

    
    ce programme est liée au contrôle formulaire "localisation"


**spg/voir.php** 

    Ce script est associé au contrôle "upload"
    
    Ce sous programme permet de visualiser un fichier téléchargé
    sur le serveur (pdf ou image)
    

**spg/upload.php**


        Ce script utilise la classe core/upload.class.php (composant openMairie)

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

Les méthodes de core/om_formulaire.class.php peuvent être surchargées dans obj/om_formulaire.class.php

Les scripts javascript de js/script.js peuvent être surchargés dans app/js/script.js

Les méthodes de core/om_dbform.class.php peuvent être surchargées dans obj/om_dbform.class.php



=================================================================
Les nouvelles utilisations dans les objets metiers (openMairie 4)
=================================================================

openMairie4 apporte de nouvelles fonctions qu'il est utile d'implémenter dans
les objets métiers


**récuperer le type de la base** depuis l'objet db : $db->phptype (mysql ou pgsql)::


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
   