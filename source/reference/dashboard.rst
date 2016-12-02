.. _dashboard:

##################
Le tableau de bord
##################

.. contents::

========
Principe
========

Il est proposé dans ce chapitre de décrire le tableau de bord paramètrable pour
les utilisateurs

-----------
paramétrage
-----------

l'accès au tableau de bord paramètrable dyn/dashboard.inc.php

(voir framework/paramétrage)

Par défaut, le tableau de bord paramétrable est activé, il peut être déconnecté en
enlevant le commentaire // die().



-----------
les widgets
-----------

Les widgets sont des liens et/ou de petits scripts paramétrables qui peuvent être rajoutés dans
le tableau de bord. Ces scripts sont conservés dans la table om_widget.

Chaque utilisateur paramètre son tableau de bord.



-------------------------------
le tableau de bord paramétrable
-------------------------------

L'administrateur choisit les widgets présents sur le tableau de bord de chaque profil parmi ceux proposés dans l'application. Il peut placer les widgets où il le souhaite.





======
widget
======

Le widget (WIDGET DASHBOARD) est un bloc d'informations contextualisées accessible depuis le tableau de bord de l'utilisateur. Il peut être de type 'Web' ou de type 'Script'.


---------------------
la création de widget
---------------------

La saisie des widget se fait dans administration -> om_widget.


La grille de saisie est la suivante ::

    libellé du widget qui apparaitra à l adition du widget dans le tableau de bord
    lien qui sera implémenté (# : pas de lien)
    texte : texte du widget (iframe, javascript, ajax ...)
    profil : profil autorisé pour le tableau de bord





.. image:: ../_static/widget_1.png



Le tableau de bord, peut gérer toutes sortes d'informations internes ou externes à
l'application ::

    les taches non soldees pour openCourrier
    les appels à la maintenance
    l'horoscope, la météo, une vidéo, des photos ...


-----------------------
Le widget de type 'Web'
-----------------------

interne
=======

les liens sur les cartes (à mettre danbs le champ lien)::

    la carte de raphele avec tab_sig_point.php
    ../scr/tab_sig_point_db.php?obj=raphele_1&zoom=6
    celle de mas thibert :
    ../scr/tab_sig_point.php?obj=odp_6&zoom=7


les accès personnalisés "ajax"au travers de son code utilisateur (dans openCourrier) ::

    <script type='text/javascript'>
        $.ajax({
            type: 'GET',
           url:'../app/tab_wid.php',  
           cache: false,
           data: '&obj=tachenonsolde_service',
            success: function(html){
                $('#aff3').append(html);
            }
        });
    </script>
    <div id='aff3'></div>


Ce code lance dans le widget ../app/tab_wid.php?obj=tachenonsolde_service

tachenonsolde_service est initialisé dans sql/mysql/tachenonsolde_service.inc

Il ne s'affichera que la première page (paramétrer $serie pour le nombre d'enregistrement affichés)

Attention si vous affichez plusieurs widgets "openmairie", mettre un id different
pour chaque div (ici aff3)


externe
=======

Les autres applications openMairie peuvent aussi être accessibles par widget de la même
manière que le paragraphe ci dessus.


D'autres widgets externes sont accessibles en mettant dans le champ texte les
scripts suivants :


Acces à une video externe avec un "iframe" ::

    <iframe width='200' height='150'
        src='http://www.youtube.com/embed/gS5B4LlqkfI'
        frameborder='0' allowfullscreen>
    </iframe>

La meteo grace à un javascript du site tameteo.com ::

    <div id='cont_f5089b722555454d1872b91f52beafd4'>
        <h2 id='h_f5089b722555454d1872b91f52beafd4'>
        <a href='http://www.tameteo.com/' title='Météo'>Météo</a></h2>
        <a id='a_f5089b722555454d1872b91f52beafd4'
            href='http://www.tameteo.com/
                        meteo_Arles-Europe-France-Bouches+du+Rhone--1-25772.html'
            target='_blank' title='Météo Arles'
            style='color:#666666;font-family:1;font-size:14px;'></a>
        <script type='text/javascript'
            src='http://www.tameteo.com/wid_loader/f5089b722555454d1872b91f52beafd4'>
        </script>
    </div>



Horoscope au travers d un iframe qui pointe sr astroo.com ::

    <!--DEBUT CODE ASTROO-->
    <!--debut code perso-->
    <iframe width='232' height='302' marginheight='0' marginwidth='0' frameborder='0'
        align='center' src='http://www.astroo.com/horoscope.htm'
        name='astroo' allowtransparency='true'>
    <!--fin code perso-->
    <a href='http://www.astroo.com/horoscope.php' target='_top'
        title='Cliquez-ici pour afficher l'horoscope quotidien'>
        <font face='Verdana' size='2'><b>afficher l'horoscope du jour</b>
        </font></a>
    </iframe>
    <noscript>
    <a href='http://www.astroo.com/horoscope.php' target='_blank'>horoscope</a>
    </noscript>
    <!--FIN CODE ASTROO-->

Acces à un fil rss avec un module ajax google ::

    <script src='http://www.gmodules.com/ig/ifr?url=
       http://www.ajaxgaier.com/iGoogle/rss-reader%2B.xml
       &up_title=Actualit%C3%A9s%20atReal
       &up_feed=http%3A%2F%2Fwww.atreal.fr%2Fatreal%2Fcommunaute%2Factualites-atreal%2FRSS
       &up_contentnr=9&up_fontsize=9&up_lineheight=70
       &up_titlelink=&up_bullet=1
       &up_reload_feed=0&up_reload_fqcy=0
       &up_hl_background=FFFFFF&synd=open&w=200&h=100
       &title=
       &border=%23ffffff%7C3px%2C1px+solid+%23999999&output=js'>
    </script>


Affichage de photos avec flick 'r (appel javascript)::

    <table><tr>
    <div class='flick_r'>
    <script type='text/javascript'
        src='http://www.flickr.com/badge_code_v2.gne?count=3
            &display=latest&size=s
            &layout=h&source=user
            &user=27995901%40N03'></script>
    </div>
    </tr></table>


--------------------------
Le widget de type 'Script'
--------------------------

``app/widget_example.php``

.. code-block:: php

   <?php
   /**
    * WIDGET DASHBOARD - widget_example.
    *
    * L'objet de ce script est de fournir un exemple de widget de type 'Script'.
    *
    * @package openmairie_framework
    * @version SVN : $Id$
    */
   
   // On instancie la classe utils uniquement si la variable $f n'est pas déjà définie
   // pour protéger l'accès direct au script depuis l'URL. La permission "forbidden"
   // a pour vocation de n'être donnée à aucun utilisateur.
   require_once "../obj/utils.class.php";
   if (!isset($f)) {
       $f = new utils(null, "forbidden");
   }
   
   //
   $footer = "";
   
   //
   $footer_title = "";
   
   //
   $widget_is_empty = true;
   
   ?>


-----------------
Modèle de données
-----------------

.. code-block:: sql

    CREATE TABLE om_widget
    (
      om_widget integer NOT NULL, -- Identifiant unique
      libelle character varying(100) NOT NULL, -- Libellé du widget
      lien character varying(80) NOT NULL DEFAULT ''::character varying, -- Lien qui pointe vers le widget (peut être vers une URL ou un fichier)
      texte text NOT NULL DEFAULT ''::text, -- Texte affiché dans le widget
      type character varying(40) NOT NULL, -- Type du widget ('web' si pointe vers une URL ou 'file' si pointe vers un fichier)
      CONSTRAINT om_widget_pkey PRIMARY KEY (om_widget)
    );

- ``obj/om_widget.class.php``
- ``sql/pgsql/om_widget.form.inc.php``
- ``sql/pgsql/om_widget.inc.php``
- ``core/obj/om_widget.class.php``
- ``core/sql/pgsql/om_widget.form.inc.php``
- ``core/sql/pgsql/om_widget.inc.php``
- ``gen/obj/om_widget.class.php``
- ``gen/sql/pgsql/om_widget.form.inc.php``
- ``gen/sql/pgsql/om_widget.inc.php``


====================
Les tableaux de bord
====================

------------------------
accès au tableau de bord
------------------------

Le paramétrage se fait en cliquant sur le lien "paramétrer son tableau de bord"

Il apparait alors ::

    un "plus"  pour ajouter un widget pour une colone
    une croix pour supprimer un widget
    
Le déplacement du widget de haut en bas ou de gauche à droite se fait par copier/glisser avec la souris.



.. image:: ../_static/tdb_1.png


En cliquant sur "+", il est possible de rajouter des widgets dans son tableau de
bord

.. image:: ../_static/tdb_2.png

-----------------
Modèle de données
-----------------

.. code-block:: sql

    CREATE TABLE om_dashboard
    (
      om_dashboard integer NOT NULL, -- Identifiant unique
      om_profil integer NOT NULL, -- Profil auquel on affecte le tableau de ville
      bloc character varying(10) NOT NULL, -- Bloc de positionnement du widget
      "position" integer, -- Position du widget dans le bloc
      om_widget integer NOT NULL, -- Identifiant du widget
      CONSTRAINT om_dashboard_pkey PRIMARY KEY (om_dashboard),
      CONSTRAINT om_dashboard_om_profil_fkey FOREIGN KEY (om_profil)
          REFERENCES openexemple.om_profil (om_profil),
      CONSTRAINT om_dashboard_om_widget_fkey FOREIGN KEY (om_widget)
          REFERENCES openexemple.om_widget (om_widget)
    );

- ``obj/om_dashboard.class.php``
- ``sql/pgsql/om_dashboard.form.inc.php``
- ``sql/pgsql/om_dashboard.inc.php``
- ``core/obj/om_dashboard.class.php``
- ``core/sql/pgsql/om_dashboard.form.inc.php``
- ``core/sql/pgsql/om_dashboard.inc.php``
- ``gen/obj/om_dashboard.class.php``
- ``gen/sql/pgsql/om_dashboard.form.inc.php``
- ``gen/sql/pgsql/om_dashboard.inc.php``


==========
Composants
==========

Les scripts du framework qui s'occupent de la gestion du tableau de bord sont :

- ``scr/dashboard.php``
- ``scr/dashboard_composer.php``
- ``spg/widgetctl.php``

