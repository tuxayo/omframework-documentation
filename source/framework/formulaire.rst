.. _formulaire:

###############
Les formulaires
###############

La gestion des formulaires se base sur deux fichiers :
    - core/om_formulaire.class.php (classe formulaire)
    - core/om_dbform.class.php (classe dbform)

om_formulaire.class.php permet la gestion de l'affichage, om_dbform.class.php permet la gestion de la logique interne.

La classe formulaire fait appel à des sous programmes génériques pour certains
controles au travers de script js/script.js

Les classes métier étendent de dbform, c'est donc dans ces classes que les formulaires seront préparés via l'appel aux méthodes présentes dans la classe formulaire.

Pour chaque formulaire de classe créer, le script sql/DBTYPE/[table].form.inc.php correspondant est appelé. Il permet de stocker le detail des requètes necessaires à l'affichage du contenu du formulaire.

Les formulaires sont construits lors de l'appel aux scripts scr/form.php et scr/sousform.php. En fonction des paramètres passés à l'url, un formulaire de la classe métier correspondante sera créé.

.. _script-form-sousform:

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

Les méthodes de core/om_formulaire.class.php peuvent être surchargées dans obj/om_formulaire.class.php

***********************************
Description de la classe formulaire
***********************************

.. class :: formulaire($unused = NULL, $validation, $maj, $champs = array(), $val = array(), $max = array())

   Cette classe permet une gestion complète de l'affichage d'un formulaire.

.. _méthodes-affichage-widget:

Méthodes d'affichage de widgets
===============================

Les widgets sont des éléments de formulaire, ils sont composés d'un ou plusieurs champs. Chaque méthode permet d'afficher un seul widget.

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

.. _méthodes-construction-formulaire:

Les  méthodes de construction et d'affichage
============================================

Le formulaire est constitué de div, fieldset et de champs les méthodes suivante permettent une mise en page structuré.

    .. method:: formulaire.entete()

       ouverture du conteneur du formulaire.

    .. method:: formulaire.enpied()

       fermeture du conteneur du formulaire.

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
================================================================================

Ces méthode sont appelées depuis les classes métier, elles permettent la configuration du formulaire.

    .. method:: formulaire.setType()

       type de champ

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

.. _class-dbform:

*******************************
Description de la classe dbform
*******************************

.. class:: dbform($id, &$db, $DEBUG = false)

   Cette classe est centrale dans l'application. Elle est la classe parente de chaque objet métier.
   Elle comprend des méthodes de gestion (initialisation, traitement, verification, trigger) des valeurs du formulaire.
   Elle fait le lien entre la base de données et le formulaire.
   Elle contient les actions possibles sur les objets (ajout, modification,suppression, consultation).

.. _action-consultation:

Consultation
============

La consultation (action=3) d'un élément est construite de la même façon qu'un formulaire. Elle contient une liste d'actions contextuelles configurable. Les données ne sont pas éditable.

.. image:: ../_static/mode_consultation.png
   :height: 380
   :width: 800

Par defaut, depuis les tableau, les éléments sont accessible en consultation, il est possible de les éditer à partir du menu des actions contextuelles.

Ajout
=====

L'ajout (action=0)  permet l'éditions de données. Lors de la validation, un traitement (:ref:`setValFAjout<setValFAjout>`) et une verification (:ref:`verifierAjout<verifierAjout>`) sont effectués en plus des méthodes de validation standard (:ref:`setValF<setValF>`, :ref:`verifier<verifier>`). Si la clé primaire de la table est automatique alors elle est générée.

Modification
============

L'ouverture d'un élément en modification (action=1) permet l'éditions de données déjà existantes, lors de la validation du formulaire les données sont traitées, vérifiées puis envoyées dans la base.

Suppression
===========

Accessible depuis la liste des actions contextuelles, une confirmation est demandée pour chaque suppression.

Presentation des méthodes de la classe
======================================

Les méthodes de dbform peuvent être surchargées dans obj/om_dbform.class.php

Méthodes d'initialisation de l'affichage du formulaire
------------------------------------------------------

  .. method:: dbform.formulaire($enteteTab, $validation, $maj, &$db, $postVar, $aff, $DEBUG = false, $idx, $premier = 0, $recherche = "", $tricol = "", $idz = "", $selectioncol = "", $advs_id = "", $valide = "", $retour = "", $actions = array(), $extra_parameters = array())

     Méthode d'initialisation de l'affichage de formulaire.

  .. method:: dbform.sousformulaire($enteteTab, $validation, $maj, &$db, $postVar, $premiersf, $DEBUG, $idx, $idxformulaire, $retourformulaire, $typeformulaire, $objsf, $tricolsf, $retour= "", $actions = array())

     Méthode d'initialisation de l'affichage de sous formulaire.

Cette méthode créer un objet om_formulaire et initialise certains de ces attributs via les méthodes suivantes :

  .. method:: dbform.setVal(&$form, $maj, $validation, &$db, $DEBUG = false)

     Permet de définir les valeurs des champs

  .. method:: dbform.setType(&$form, $maj)

     Permet de définir le type des champs

  .. method:: dbform.setLib(&$form, $maj)

     Permet de définir le libellé des champs

  .. method:: dbform.setTaille(&$form, $maj)

     Permet de définir la taille des champs

  .. method:: dbform.setMax(&$form, $maj)

     Permet de définir le nombre de caractère maximum des champs

  .. method:: dbform.setSelect(&$form, $maj, $db, $DEBUG = false)

     Méthode qui effectue les requêtes de configuration des champs select

  .. method:: dbform.setOnchange(&$form, $maj)

     Permet de définir l'attribut "onchange" sur chaque champ

  .. method:: dbform.setOnkeyup(&$form, $maj)

     Permet de définir l'attribut "onkeyup" sur chaque champ

  .. method:: dbform.setOnclick(&$form, $maj)

     Permet de définir l'attribut "onclick" sur chaque champ

  .. method:: dbform.setGroupe(&$form, $maj)

     Permet d'alligner plusieurs champs (obsolète depuis la version 4.3.0)

  .. method:: dbform.setRegroupe(&$form, $maj)

     Permet de regrouper les champs dans des fieldset (obsolète depuis la version 4.3.0)

  .. method:: dbform.setLayout(&$form, $maj)

     Méthode de mise en page, elle permet de gérer la hierarchie d'ouverture et fermeture des balises div et fieldset.


Méthodes d'actions
------------------

Ces méthodes sont appelées lors de la validation du formulaire.

  .. method:: dbform.ajouter($val, &$db = NULL, $DEBUG = false)

     Cette méthode permet l'insertion de données dans la base, elle appelle toutes les méthodes de traitement, vérification et action  des données retournées par le formulaire

  .. method:: dbform.modifier($val = array(), &$db = NULL, $DEBUG = false)

     Cette méthode permet la modification de données dans la base, elle appelle toutes les méthodes de traitement et vérification des données retournées par le formulaire

  .. method:: dbform.supprimer($val = array(), &$db = NULL, $DEBUG = false)

     Cette méthode permet la suppression de données dans la base, elle appelle toutes les méthodes de traitement et vérification des données retournées par le formulaire

Méthodes appelées lors de la validation
---------------------------------------

.. _setValFAjout:

  .. method:: dbform.setValFAjout($val = array())

     Méthode de traitement des données retournées par le formulaire (utilisé lors de l'ajout)

.. _setValF:

  .. method:: dbform.setvalF($val = array())

     Méthode de traitement des données retournées par le formulaire

.. _verifier:

  .. method:: dbform.verifier($val = array(), &$db = NULL, $DEBUG = false)

     Méthode de verification des données et de retour d'erreurs

.. _verifierAjout:

  .. method:: dbform.verifierAjout($val = array(), &$db = NULL)

     Méthode de verification des données et de retour d'erreurs (utilisé lors de l'ajout)

  .. method:: dbform.setId(&$db = NULL)

     Initialisation de la cle primaire (si cle automatique lors de l'ajout)

  .. method:: dbform.triggerajouter($id, &$db = NULL, $val = array(), $DEBUG = false)

     Permet d'effectuer des actions avant l'insertion des données dans la base

  .. method:: dbform.triggerajouterapres($id, &$db = NULL, $val = array(), $DEBUG = false)

     Permet d'effectuer des actions après l'insertion des données dans la base

  .. method:: dbform.triggermodifier($id, &$db = NULL, $val = array(), $DEBUG = false)

     Permet d'effectuer des actions avant la modification des données dans la base

  .. method:: dbform.triggermodifierapres($id, &$db = NULL, $val = array(), $DEBUG = false)

     Permet d'effectuer des actions après la modification des données dans la base

  .. method:: dbform.triggersupprimer($id, &$db = NULL, $val = array(), $DEBUG = false)

     Permet d'effectuer des actions avant la modification des données dans la base

  .. method:: dbform.triggersupprimerapres($id, &$db = NULL, $val = array(), $DEBUG = false)

     Permet d'effectuer des actions après la modification des données dans la base

.. _setLayout:

Mise en forme et configuration des formulaires
==============================================

La configuration des formulaires se fait via les scripts sql/type_de_sgbd/nom_objet.form.inc.php

Dans ces scripts, peuvent être surchargé, la liste des champs (ordre ou champs affichés), requêtes sql permettant de remplir les widget de formulaires ainsi que les actions du menu contextuel.

L'ajout d'une action se presente de cette façon :

.. code-block:: php

   <?php
   $portlet_actions['edition'] = array(
       'lien' => '../pdf/pdflettretype.php?obj=om_utilisateur&amp;idx=',
       'id' => '',
       'lib' => '<span class="om-prev-icon om-icon-16 om-icon-fix pdf-16" title="'._('Edition').'">'._('Edition').'</span>',
       'ajax' => false,
       'ordre' => 21,
   );
   ?>

Depuis la version 4.3.0 les formulaires sont composés de div et non plus d'une hierarchie de table. Il est donc possible d'organiser les blocs de champ plus facilement.

Exemple de formation d'un widget de formulaire :

.. code-block:: html

   <div class="field field-type-text">
       <div class="form-libelle">
           <label class="libelle-nom" for="nom">
               nom <span class="not-null-tag">*</span>
           </label>
       </div>
       <div class="form-content">
           <input type="text" class="champFormulaire" maxlength="30" size="30" value="Administrateur" id="nom" name="nom">
       </div>
   </div>


La mise en forme se fait via la methode setLayout() dans chaque surcharge de la classe dbform. Elle permet de gérer la hierarchie d'ouverture et fermeture des balises div et fieldset avec les méthodes :
- setBloc($champ, $contenu, $libelle = '', $style = '') \: permet d'ouvrir/fermer ($contenu=D/F) une balise div sur un champ ($champ), avec un libellé ($libelle) et un attribut class ($style).
    - une liste de classes css pour fieldset est disponible :
        - group : permet une mise en ligne des champs contenu dans le div
        - col_1 à col_12 : permet une mise en page simplifiée, par exemple : "col_1" permet de définir une taille dynamique de 1/12ème de la page , col_6 correspond à 6/12 soit 50% de l'espace disponible.
    - il est possible de créer et ajouter des classes css aux différents div afin d'obtenir une mise en page personalisé.
- setFieldset($champ, $contenu, $libelle = '', $style = '') \: permet d'ouvrir/fermer ($contenu=D/F) un  fieldset sur un champ ($champ), avec une legende ($libelle) et un attribut class ($style).
    - une liste de classes css pour fieldset est disponible :
        - collapsible : ajoute un bouton sur la legende (jQuery) afin de refermer le fieldset.
        - startClosed : idem à la difference que le fieldset est fermé au chargement de la page.
- exemple d'implémentation de la méthode setLayout() afin d'obtenir le même affichage sans utiliser les méthodes setGroupe() et setRegroupe() :

  .. code-block:: php

    <?php
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
      ?>



******************************
Les sous programmes génériques
******************************

Les sous programmes génériques sont des sous programmes associés aux contrôles
du formulaire et appellés par eux par un script js dans js/formulairedyn.js.

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
    
Ce sous programme permet de visualiser un fichier téléchargé sur le serveur (pdf ou image)
    

**spg/upload.php**


Ce script utilise la classe core/upload.class.php (composant openMairie)

Le paramétrage des extensions téléchargeables se fait dans le fichier autorise dans dyn/config.inc.php


**spg/rvb.php** et js/rvb.js


Ce script est associé au contrôle "rvb" et permet l'accès à une palette de couleur
pour récupérer un code couleur rvb


*****************************************************************
Les nouvelles utilisations dans les objets metiers (openMairie 4)
*****************************************************************

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