.. _geocodage:

#########
geocodage
#########


Ce chapitre est consacré au problème de géocodage.


Il est propose un géocodage interne (sgbd adresse en reseau interne) ou un geocodage externe

Il convient de regarder les termes de licences concernant les API externes non libres
(mapquest/osm) afin de s'assurer de bien respecter les obligations de l'autorisation
gratuite.

Un document décrivant les contraintes juridiques et techniques de l'utilisation des API
est accessible au lien suivant ::

http://www.openmairie.org/communautes/groupe-de-travail-sig-adullact/comparatif-api.pdf/view


Le parametrage se fait dans le fichier sig/var_adresse_postale.php


var_adresse_postale.inc
=======================

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
===================================================================

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



