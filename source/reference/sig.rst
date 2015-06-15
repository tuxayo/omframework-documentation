.. _sig:

########################
Module 'Géolocalisation'
########################



Il est necessaire que l'API openLayers soit dans le framework :

lib/openlayers



========
Principe
========


Il est proposé dans ce chapitre de decrire le module
tab_sig.php qui permet la geo localisation d'objet dans openMairie


A partir de la version 4.4.0, l'accès au module géographique se fait en mettant l'option_localisation d'om_paramètre à la valeur "sig_interne"
    

L'objectif de tab_sig_map est de permettre une saisie le plus souvent automatique 
par un point, ligne, multiligne, polygone, multipolygone. Cette saisie est  stockée dans la base métier postgresql.
Elle est affichée sur des fonds existants sur internet : google sat, openStretmap ou bing (pour l instant) en utilisant le composant javascript openLayers

Il n'est donc pas nécessaire de disposer d'un SIG pour utiliser tab_sig.php.

Le format de stockage des données pgsql est celui de l'OGC et il est accessible aux
clients libres où propriétaires qui respectent ce format
(QGIS, GRASS, VEREMAP  ... pour les clients libres)


Géo localisation automatique
----------------------------

L'enjeu est de limiter au maximum la géo localisation manuelle dès
qu'il y a une possibilité de géo localisation automatique.

Elle se fait au travers de 2 programmes (voir paragraphe sur le geocodage):

- adresse_postale.php : positionnement suivant le numero et rue

- adresse_postale_google.php : positionnement suivant le numero et rue avec google

- adresse_postale_bing.php : positionnement suivant le numero et rue avec bing

- adresse_postale_mapquest.php : positionnement suivant le numero et rue avec mapquest


La géolocalisation automatique peut se faire sur une base externe
postgresql (eventuellement via une vue)

le script tab_sig.php permet de saisir manuellement le point.



Affichage de carte
------------------

L'affichage se fait avec openLayers dont le composant est de base
dans le framework openMairie : lib/openLayers. (le composant est
installé de manière a être optimisé avec une css openmairie)

La librairie proj4 inclus dans lib/openLayers permet de pouvoir utiliser
les projections lambert sud et lambert 93.

La projection géographique et Mercator est de base dans openLayers

L'enjeu est donc de projeter les données stockées dans la base "métier"
postgresql - postgis (les communes devant utiliser le lambert93) en mercator
pour être lisible avec les cartes accessibles sur internet.

L'affichage des datas est fait au travers d'une requête postgresql
qui alimente un tableau json lu comme une couche openLayers.

La data à modifier est fourni par requete postgresql au format wkt à openLayers.
(voir paragraphe layers)

tab_sig.php permet ::

    - l affichage de/des  fond(s)
    - l'affichage de données (data)
    - l affichage du geométries qui peut être créé ou déplacé (couche wkt)

dans la version 4.2.0, tab_sig permet aussi ::

    - l'affichage de flux wms et wfs (getmap) et de recuperer les données (getfeature)
    - la collation de géométrie dans un pannier et son enregistrement en multi géométries




Paramétrage de la carte
-----------------------

Le paramétrage général (contenu dans scr/tab_sig.php) des cartes  est modifiable dans 
dyn/var_sig.inc ::

    // *** parametre de tab_sig.php ***
    // generer une cle pour le site : http://code.google.com/intl/fr/apis/maps/signup.html
    $cle_google = "";
    $fichier_jsons="json_points.php?obj=";
    $fichier_wkt="wkt_point.php";
    //zoom par couche : zoom standard permettant un passage de zoom a l autre
    $zoom_osm_maj=18;
    $zoom_osm=14;
    $zoom_sat_maj=8;
    $zoom_sat=4;
    $zoom_bing_maj=8;
    $zoom_bing=4;
    // popup data contenuHTML
    $width_popup=200;
    $cadre_popup=1;
    $couleurcadre_popup="black";
    $fontsize_popup=12;
    $couleurtitre_popup="black";
    $weightitre_popup="bold";
    $fond_popup="yellow";
    $opacity_popup="0.7";
    // image localisation maj ou consultation
    $img_maj="img/punaise.png";
    $img_maj_hover="img/punaise_hover.png";
    $img_consult="img/punaise_point.png";
    $img_consult_hover="img/punaise_point_hover.png";
    $img_w=14;
    $img_h=32;
    $img_click="1.3";// multiplication hauteur et largeur image cliquee
    
    // *** parametres d om_sig_map.class.php, om_sig_wms.class.php
    $contenu_etendue[0]= array('4.5868,43.6518,4.6738,43.7018',
                              '4.701,43.3966,4.7636,43.4298',
                              '4.71417,43.64,4.72994,43.65166',
                              '4.72345,43.55348,4.73134,43.55932',
                              '5.2094,43.4136,5.3345,43.4759'
                              );
    $contenu_etendue[1]= array('agglomeration',
                              'salin de giraud',
                              'raphele',
                              'Mas thibert',
                              'vitrolles'
                              );
    $contenu_epsg[0] = array("","EPSG:2154","EPSG:27563");
    $contenu_epsg[1] = array("choisir la projection",'lambert93','lambertSud');
        

La version 4.4.0 contient les étendues des communes des bouches du rhône.

=========
objet map
=========


ce chapitre propose de décrire l'utilisation de l'objet map
d'openLayers dans tab_sig.php.

Cet objet permet de définir

- le div ou la carte sera affichée (dans tab_sig.php la carte s'affiche avec le div : map-Id)

- les options de la carte et les controles affichés


===================
afficher les layers
===================


ce chapitre propose de décrire l'utilisation de l'objet layers
d'openLayers dans tab_sig.php.

.. image:: ../_static/tab_sig.png 


Dans le lien, il est possible de définir ::

- la  carte a afficher suivant l'id : ?obj=   Obligatoire
- le fond affichable par défaut : sat, bing, osm : &fond =
- l'étendue : &etendue =
- l enregistrement à modifier : &idx=

Les cartes sont paramétrées dans om_sig_map (menu administration)

.. image:: ../_static/om_sig_map_form.png 

Il est possible de copier une carte et de paramétrer  les champs suivants::

    - id : identifiant unique (obligatoire)
    - libelle
    - fonds a afficher et data (osm, bing, sat(google))
    - étendue et epsg (voir sig/var_sig_point.inc)
    - url (qui pointe sur la fiche ou le formulaire de saisie)
    - requete sql qui affiche les données json et qui doit désigné :
        le titre
        la description
        l idx
    - la mise a jour si oui,
                le champ géometrique et la table maj,
                le type de géometrie et le nom de la couche openLayers (version 4.2.0) 
    - le retour de la carte



Dans tab_sig.php, il y a 3 types de layers :

- les fonds de cartes existants sur internet (base layers)
 
- les données issus de postgresql (overlays)

- les données wms (overlay)


Les fonds
---------

Il est proposé les fonds suivants :

osm : openstreetmap

sat : satelite google 

bing : satellite microsoft 

Les datas
---------

Information de la carte : layer_info 

Cette couche fait appel à sig_json.php

Il est possible de faire appel a un autre script (voir dyn/var_sig.inc)

La requête pgsql est paramétrée dans la table om_sig_map et doit définir les champs
geom, titre, description et texte.

.. image:: ../_static/tab_sig_json.png 


sig_json.php présente tous les enregistrements d'un même
point (même géom) sur un  seul popup

En effet, il est constitué un popup lorsque l on clique sur l objet
et donne la possibilité à un accès URL parametrée dans om_sig_map

Les flux wms
------------

Le paramètrage des flux wms est saisi dans om_sig_wms

.. image:: ../_static/om_sig_wms_form.png 

il faut saisir ::

    - libelle du champ
    - la collectivité
    - l'identifiant (il doit être unique pour chaque couche wms)
    - le lien de la couche (http)
    - les layers de la couches séparés par une virgule
    
Exemple de lien avec qgis serveur ::

    http://localhost/cgi-bin/qgis_mapserv.fcgi
        ?SERVICE=WMS&VERSION=1.3.0
        &map=/var/www/openfoncier/trunk/app/qgis/openfoncier.qgs


L'affectation des flux wms dans une carte est saisi dans om_sig_map_wms

Il est saisi ::

    le nom du flux wms
    nom du layer sur la carte
    l ordre d affichage
    la visibilité par défaut (case à cocher)
    

.. image:: ../_static/om_sig_map_wms_form.png 


Sur la carte ci dessous le flux wms est activé et affiche le lotissement (getMap)

En cliquant sur le lotissement, il est possible d'accéder aux données (getFeature)

.. image:: ../_static/tab_sig_wms.png 


version 4.4.0

Trois nouveaux paramètres sont disponnibles :

sqlfilter : possibilité de filtre du flux wms (attribut FILTER)

  compléter la zone avec une requête SQL qui va généré le filtre (syntaxe suivant le serveur WMS)
  
exemple d'un filtre ::

    pour produire le filtre suivant :
    layer1:"champ1" = 'valeur1',layer2:"champ2" = 'valeur2'
    
    il faut entrer la requête suivante pour selectionner les electeurs d'un bureau :
    
    select 'electeur:²bureau² = '''||bureau.bureau||''' as buffer from &DB_PREFIXEbureau where bureau = '&idx'
    
    
    select 'electeur:²bureau² = ''&idx'' as buffer from &DB_PREFIXEbureau where bureau = '&idx'
    
    ² = caractère utilisé pour les doubles quotes : "
    || concatenation sql
    ''' permet d echapper la simple quote
    '' sql remplace les deux quotes par une quote (caractere quote)

    le filtre final appliqué au flux wms est : electeur:"bureau" = '04'  pour le bureau 04
     

base_layers : possibilité d'utiliser le flux wms comme base layers (au même niveau qu'OSM)

single_tile : raméne le flux wms en une seule image pour la fenêtre et non en imagette
(permet de corriger les labels tronqués)

Attention les temps de réponses peuvent s'allonger car il n'y a pas de cache.

La notion de pannier
--------------------

Le pannier permet de pouvoir stocker des géométries au travers de flux wms mais attention, la géométrie est
récupérée dans une table ou une vue postgis (c'est pour l'instant une limite de la version 4.2.0)

exemple : openFoncier carte dossier :

Il est proposé dans ce cas de stocker des polygones dans le pannier et de sauvegarder un multipolygone
constitué de ces polygones récupérés dans le pannier

Choisir dans le select "polygone"; L'etat est "dessinner"

Il apparait le pannier "parcelle". Sélectionner les parcelles en cliquant dessus (elles sont vertes)

.. image:: ../_static/tab_sig_pannier1.png 

Valider une fois les parcelles choisies (elles deviennent rouge)

.. image:: ../_static/tab_sig_pannier2.png 

Appuyer sur "enregistrer", l'état devient enregistrer

.. image:: ../_static/tab_sig_pannier3.png 


Cliquer sur le jeu de parcelles de votre choix (ce jeu devient vert clair)


Il peut y avoir un ou plusieurs panniers : exemple : parcelle, batiment. par contre la géométrie récupérée ne
concerne qu une seule couche



la gestion de pannier se fait dans om_sig_map_wms ::


    panier :        option pannier activé (Oui/non)         Exemple dossier/openFoncier :
    pa_nom :        nom du pannier                          parcelle
    pa_layer :      nom du layer pannier                    parcelle
    pa_attribut:    attribut de la couche à récupérer       parcelle
    pa_encaps:      caractère d'encapsuation (la ')         '
    pa_sql:         requête de récupération                 select astext(st_union(geom)) as geom
                                                            from &DB_PREFIXEparcelle where parcelle in (&lst) 
    pa_type_geometrie:  type de géométrie                   polygone



le script de gestion de pannier est : scr/sig_pannier.php



La géométrie à modifier : couche vectors :
------------------------------------------

Le chargement de la couche vectors se fait si dans la table om_sig_map,
la case maj est activée. 

La géométrie est récupérée par le script sig_wkt.php (appel a un script paramètrable dans var_sig.inc)
et la carte est centrée sur la géométrue

Il est possible de :
    
    - positionner manellement la géométrie
    - déplacer la géométrie
    - enregistrer la géometrie  : selectionner la géométrie, le programme
        form_sig.php est chargé en fenetre et permet de supprimer
        la géométrie (champ geometrique = null)  ou modifier cette géométrie.
    
    Les fonctions javascript et les controles sont activées suivant chaque état.
   
Dans dyn/form_sig_update.inc.php, il est possible de paramétrer des post traitements de saisie

Dans dyn/form_sig_delete.inc.php, il est possible de paramétrer des post traitements de suppression


Les géométries complémentaires
------------------------------

Il peut y avoir plusieurs géométries pour un même objet.

Elles sont saisies dans om_sig_map_comp ::

    titre               polygone    nom de la nouvelle géométrie
    ordre d affichage   1           ordre d'affichage dans le select
    actif               coché       activé la nouvelle géométrie
    Mise a jour         coché       autorisé la mise à jour
    type de géométrie   polygone    polygone, point, ligne
    table               dossier     table du champ géométrique
    champ               geom1       champ géometrique concerné

.. image:: ../_static/om_sig_map_comp_form.png 


Dans l exemple précédent, il apparait une fenêtre select ou l utilisateur a le choix entre une géométrie "point"
et une géométrie "polygone" du fait de la mise en place d'une géométrie complémentaire.



   
=========================
installation d'om_sig_map
=========================

Pour faire fonctionner tab_sig.php, il faut :

- installer postgis sous postgres

- openlayers qui est de base dans le framework lib/openlayers


optimisation composant openLayers
---------------------------------

construire un OpenLayers.js compresse dans le repertoire build ::

    $ cd buill
    $ python build.py 

le fichier fait 800 ko au lieu de 3 Mo



- compression lite ::

    $ python build.py lite.cfg
    le fichier fait 120 ko
    regarder dans le fichier "lite" les fichiers qui sont inclus
    et éventuellement le compléter



=======
postgis
=======

ce chapitre propose de décrire les possibilités d'utilisation de postgis dans openMairie.


principes
---------

Il est proposé un renvoi sur la documentation française.

http://postgis.refractions.net/documentation/manual-1.3/ch06.html 

ou sur le projet pédagogique postgis (conférence avec documentation et nombreux exemples :

https://adullact.net/projects/postgis/

Les requêtes utilisant des fonctions potgis peuvent être implémentées dans "reqmo"


Il sera proposé un lien sur un tutorial utilisant les fonctions postgis.


base et schéma
--------------

Il est noté que les applications openMairie peuvent s'installer dans un schéma.

Les tables et fonctions postgis sont alors accessible dans le schéma public.



=========
geocodage
=========


Ce chapitre est consacré au problème de géocodage.


Il est propose un géocodage interne (sgbd adresse en reseau interne) ou un geocodage externe

Il convient de regarder les termes de licences concernant les API externes non libres
(mapquest/osm) afin de s'assurer de bien respecter les obligations de l'autorisation
gratuite.

Un document décrivant les contraintes juridiques et techniques de l'utilisation des API
est accessible via `ce lien`_.

.. _ce lien: http://www.openmairie.org/communautes/groupe-de-travail-sig-adullact/comparatif-api.pdf/view

Le parametrage se fait dans le fichier sig/var_adresse_postale.php


var_adresse_postale.inc
-----------------------

paramètrage général ::

    $longueurRecherche=1;


adresse postale stockée sur une base dans le réseau interne ::

    // table et champs de la requete adresse postale ou l information doit etre recupere
    $t="adresse_postale";                       // table adresse postale
    $t_voie = "rivoli";                         // code adresse 
    $t_numero="num_voi";                        // numero dans la voie
    $t_complement="suf_voi";                    // suffixe (bis, ter ...)
    $t_geom="the_geom";                         // geometry point(X,Y)
    $t_adresse="(typevoie ||' '||nomvoie)";     // libelle de l adresse
    $t_quartier="id_parc";
    // *** a voir 
    $t_cp='';                                   // nom champ cp
    $t_ville='';                                // nom champ ville
    $t_insee='';                                // nom champ insee

    // champ du formulaire ou l adresse est saisi pour retour du point de geolocalisation
    $f_numero='numero_voie';        // nom champ du numero dans la voie
    $f_voie='voie';                 // nom champ du code de la voie (rivoli) 
    $f_complement='complement';     // nom champ du complement de numero
    $f_geom='geom';                 // nom champ geometrique point(X,Y)
    $f_libelle='libelle_voie';      // nom champ libelle de la voie
    // *** a voir
    $f_cp='';                       // nom champ cp
    $f_ville='';                    // nom champ ville
    $f_insee='';                    // nom champ insee
    
    
Cas ou la table d'adresse est stockées dans une autre base (voir parametrage framework de database.inc.php)::

    $db_externe='Oui';  //  Oui : base externe
    $dsn_externe= array(
        'title'  =>"base locale des adresses de l'IGN",
        'phptype'  => "pgsql",
        'dbsyntax' => "pgsql",
        'username' => "postgres",
        'password' => "postgres",
        'protocol' => "tcp",
        'hostspec' => "localhost",
        'port'     => "5432",
        'socket'   => "",
        'database' => "ignlocal",
        'formatdate'=> "AAAA-MM-JJ",
        'schema'  => "public",
        'prefixe'  =>""
    );
    $db_option_externe=array('debug'=>2,
        'portability'=>DB_PORTABILITY_ALL);

Géolocalisation par accès à un API externe ::

    // variables par defaut cp et ville si non renseignées dans le formulaire
    // pour recherche
    $cp="13200"; 
    $ville="Arles";
    $pays = ""; // a voir
    // epsg de transformation pt adresse postale dans la base en cours
    $epsg= "EPSG:27563";
    // acces au script adresse_postale externe
    $adresse_interne="Oui";
    $google="Oui"; // google
    $bing="Oui";   // bing
    $osm="Oui";    // mapquest
    
le parametre $adresse_interne à Oui permet de consulter une adresse stiockée dans le
réseau interne sur la même base, ou sur une base différente (voir plus haut)

Ensuite 3 API peuvent être initialisés : google, bing et mapquest


Mise en oeuvre dans un formulaire d'un bouton de la geolocalisation
-------------------------------------------------------------------

La géolocalisation se fait sur la base du script ::

    sig/adresse_postale.php
    qui fait appel suivant le paramétrage à :
        adresse_postale_bing.php
        adresse_postale_google.php
        adresse_postale_mapquest.php
 
.. image:: ../_static/geocodage_1.png 


Il est appelé depuis la classe métier suivant l'exemple suivant :

Exemple de openmairie_domainepublic : objet odp ::
    
    dans sql/pgsql/odp.form.inc : le champ adressepostale est implementé comme un champ vide
    $champs=array("odp", ...
                "'' as adresse_postale",  // specific
    
    dans obj/odp.class.php 
    
    dans la methode setType, le champ adresse_postale est du type httpclick
    
        function setType (&$form, $maj) {
            parent::setType ($form, $maj);
            $form->setType('adresse_postale', 'httpclick');
    
    avec la methode setVal : valoriser par défaut l'accès au script adresse_postale
                             app/js/script.js  
        
       function setVal(&$form, $maj, $validation, &$db, $DEBUG=null){
           // bouton adresse postale
           $form->setVal("adresse_postale",
            "adresse_postale('f1',f1.libelle_voie.value,f1.numero_voie.value)");
       }
    
    Initialiser une variable globale égale à 0 et qui prend la valeur 1 si la zone geometrique
    est au format wkt
    En effet le point ramené par l API externe est au format geographique (lattitude, longitude) en wkt
    il commence par POINT(x, y) et il convient de le mettre dans la projection de la zone géometrique de la table ODP
    
        class odp extends odp_gen {
    
            var $wkt=0;    

    
    dans la methode setValF, repérer une valeur wkt
            if(substr($val['geom'],0,5)== "POINT"){
                $this->wkt=1;
                $this->valF['geom'] = null;
            } ...
            
    utiliser les methodes de mise à jour après saisie pour la geometrie :
    
        function triggermodifierapres($id,&$db,$val,$DEBUG) {
            if($this->wkt==1){
                $this->sig_wkt($id,&$db,$val,$DEBUG);
            }
        }
    
        function triggerajouterapres($id,&$db,$val,$DEBUG) {
            $id=$this->valF[odp]; // id n est pas valorise en ajout
            if($this->wkt==1){
                $this->sig_wkt($id,&$db,$val,$DEBUG);
            }
        }
    
        function sig_wkt($id,&$db,$val,$DEBUG){
            // si wkt -> saisie en format binaire wkb pour postgre
            $projection = $db -> getOne("select srid from geometry_columns where f_table_name='".
            $this->table."'");
            $sql ="update ".$this->table." set geom =geometryfromtext('".$val["geom"]."', ".
            $projection." ) where ".$this->table." ='".$id."'";
            $res = $db -> query($sql);
            if (DB :: isError($res)){
                die($res->getMessage()."erreur ".$sql);
            }else{
                $this->msg = $this->msg."&nbsp;"._("le point trouvé par l'API est sauvegardé")."&nbsp;".
                $this->table."&nbsp;".$id;
            }
        }






========
data_sig
========


ce chapitre propose de décrire la récupération de données SIG nécessaires à la mise en oeuvre
des scripts SIG internes openMairie.


Saisir le périmètre de sa commune
---------------------------------

Il s'agit d'adapter ses cartes au périmètre de sa commune

Aller sur openstreetMap  http://www.openstreetmap.org/

Chercher la ville : exemple "Gréasque"

Ajuster la carte aux frontières communales

Aller dans l onglet export et noter les coordonnées géographiques "zone à exporter"

.. image:: ../_static/osm_export.png 

Dans le fichier dyn/var_sig.inc, modifier le tableau de variables avec les coordonnées
de la manière suivante ::

    $contenu_etendue[0]= array('5.5155,43.4081,5.5781,43.4426');
    $contenu_etendue[1]= array('greasque'); 

Modifier les cartes de om_sig_point


Récupérer les données de l'IGN
------------------------------

Les bases suivantes sont fournies gratuitement aux collectivités

- base topographique

- base parcellaire

- base adresse

L'application openReferentiel (Vitrolles) permet de créer un référentiel local sur la collectivité

Les données sont fournies par département.

Il faut récupérer les fichiers shape dans une base IGN puis ensuite construire le référentiel de la commune avec openReferentiel.


Recupérer des données shape
---------------------------

Il est proposé un exemple de récupération de données "parcelle" 

Les données de l'IGN sont fournies aux communes par département.

Insérer le fichier parcelle dans la base (exemple IGN) ::

    shp2pgsql -s 2154 -I -D -W LATIN1 PARCELLE.SHP | psql ign

Il est créer dans la base "ign" une table parcelle décrite ci dessous ::

        gid serial NOT NULL,
		numero character varying(4),
		feuille smallint,
		section character varying(2),
		code_dep character varying(2),
		nom_com character varying(45),
		code_com character varying(3),
		com_abs character varying(3),
		code_arr character varying(3),
		the_geom geometry

ainsi qu un enregistrement parcelle dans géometry_columns (postgis est obligatoire
dans la base) et un index

Selectionner les parcelles de la commune concernée et inserer les dans une nouvelle table ::

    insert into parcelle_greasque 	(parcelle, section, commune, geom)
    select 	section||numero, section, code_dep||code_com, the_geom 	from parcelle
        where code_com = '046';

Pour mettre à jour le champ surface de parcelle dans openfoncier ::

    update parcelle set surface = round(cast(area2d(geom) as numeric), 2)
    

Recupération des données de la DGI
----------------------------------

Les fichiers textes de la DGI sont dans un format  récupéré dans le cadre
de l application openCadastre qui reconstitue des tables postgresql.

Les fichiers gémétriques au format EDIGEO ne sont pas récupérés compte tenu de son format non standard
et il est préféré utiliser les formats shape de l IGN

L'application openCadastre permet de récupérer les données "texte".

