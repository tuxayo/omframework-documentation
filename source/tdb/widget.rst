.. _widget:

######
widget
######


Le widget est un petit script qui s'exécute dans le tableau de bord
dans une fenêtre normalisée

Le script peut faire appel à l'application en cours ou a une application externe.

Le tableau de bord, peut gérer toutes sortes d'informations ::

    les taches non soldees pour openCourrier
    les appels à la maintenance
    l'horoscope, la météo, une vidéo, des photos ...

Nous avons mis quelques exemples dans les deux derniers paragrapĥes 


=====================
la création de widget
=====================

la saisie des widget se fait dans ::

    administration -> om_widget
    
La grille de saisie est la suivante ::

    libellé du widget qui apparaitra à l adition du widget dans le tableau de bord
    lien qui sera implémenté (# : pas de lien)
    texte : texte du widget (iframe, javascript, ajax ...)
    profil : profil autorisé pour le tableau de bord


.. image:: ../_static/widget_1.png


====================
Les widgets internes
====================

les liens sur les cartes (à mettre danbs le champ lien)::

    la carte de raphele avec tab_sig_point.php
    ../sig/tab_sig_point_db.php?obj=raphele_1&zoom=6
    celle de mas thibert :
    ../sig/tab_sig_point.php?obj=odp_6&zoom=7


les accès personnalisés "ajax"au travers de son code utilisateur (dans openCourrier) ::

    <script type='text/javascript'>
        $.ajax({
            type: 'GET',
           url:'../tdb//tab_wid.php',  
           cache: false,
           data: '&obj=tachenonsolde_service',
            success: function(html){
                $('#aff3').append(html);
            }
        });
    </script>
    <div id='aff3'></div>


Ce code lance dans le widget ../tbd/tab_wid.php?obj=tachenonsolde_service

tachenonsolde_service est initialisé dans sql/mysql/tachenonsolde_service.inc

Il ne s'affichera que la première page (paramétrer $serie pour le nombre d'enregistrement affichés)

Attention si vous affichez plusieurs widgets "openmairie", mettre un id different
pour chaque div (ici aff3)




====================
les widgets externes
====================

les autres applications openMairie


Les divers widgets externes sont accessibles en mettant dans le champ texte les
scripts suivants


acces à une video externe avec un "iframe" ::

    <iframe width='200' height='150'
        src='http://www.youtube.com/embed/gS5B4LlqkfI'
        frameborder='0' allowfullscreen>
    </iframe>

la meteo grace à un javascript du site tameteo.com ::

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



l horoscope au travers d un iframe qui pointe sr astroo.com ::

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



Jeux avec des fichiers flash
----------------------------
    
    // lancer de chaussure sur bush
    
    <!--Debut du code Kidsclae.com -->
    <div style='width:220px;font-size:x-small; text-align:center; border: 1px solid #6699cc;
    padding-top: 2px; padding-right: 2px; padding-bottom: 2px;
    padding-left: 2px; background-color:#cccccc;'>
    <noscript>
    <a href='http://www.kidsclae.com'>Kidsclae.com les jeux gratuits online</a>
    </noscript><object classid='clsid:D27CDB6E-AE6D-11cf-96B8-444553540000' codebase='http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,29,0' width='100%' height='150'>
    <param name='movie' value='http://www.kidsclae.com/downloads/chaussurebush.swf' />
    <param name='quality' value='high' />
    <embed src='http://www.kidsclae.com/downloads/chaussurebush.swf' quality='high'
    pluginspage='http://www.macromedia.com/go/getflashplayer'
    type='application/x-shockwave-flash' width='100%' height='150'>
    </embed>
    </object><br />
    <a href='http://www.kidsclae.com'>Kc</a>
    <a href='http://www.kidsclae.com/games/chaussurebush.html'>
    Lancer de chaussures sur G.W. Bush d'après le lancer du journaliste</a>
    </div>
    <!--Fin du code Kidsclae.com-->"
    
    
    //faire marcher un soulo
    
    <div id='lecteurVideo'>
    <object type='application/x-shockwave-flash'
    data='http://www.kamaz.fr/fichiers/swf/122417099822.swf' width='220' height='150'>
    <param name='movie' value='http://www.kamaz.fr/fichiers/swf/122417099822.swf' />
    <param name='allowFullScreen' value='true' />
    <param name='wmode' value='transparent' />
    <p>Mec bourré : Faites marcher le mec bourré le plus longtemps possible... au début ça va, mais la fin est bien difficile!!!</p>
    </object>
    </div>"
    
    
    
    
    // jouer de la guitare avec "guitar hero II"
    
    
    "<div style='clear:both;'>
    <object style='float:left;clear:both;' width='220' height='18'>
    <param name='movie'
    value='http://www.multigames.com/e/?t=gh&cid=EKn57X5O&tfu=Guitar_Hero_II'>
    </param>
    <param name='wmode' value='transparent'></param>
    <embed src='http://www.multigames.com/e/?t=gh&cid=EKn57X5O&tfu=Guitar_Hero_II'
    type='application/x-shockwave-flash' wmode='transparent'
    width='220' height='18'></embed>
    </object>
    <div style='float:left;clear:both;background-color:black;width:220px;height:150px;
        border-bottom:1px solid #555555;border-left:1px
        solid #555555;border-right:1px solid #555555;'>
    <object width='220' height='150'>
    <param name='movie' value='http://www.multigames.com/e/?t=g&cid=EKn57X5O&eid=1305727200-MXB81p0J'>
    </param>
    <embed src='http://www.multigames.com/e/?t=g&cid=EKn57X5O&eid=1305727200-MXB81p0J'
    type='application/x-shockwave-flash' width='220' height='150'>
    </embed>
    </object>
    </div>
    </div>
    <div style='clear:both;'><a href='http://www.multigames.com' target='_blank'
    style='font-size:11px;'>Play more free flash games at Multigames.com</a>
    </div>"



