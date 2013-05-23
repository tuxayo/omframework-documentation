.. _arborescence:

############
Arborescence
############

Cette rubrique vise à décrire brièvement l'arborescence du framework pour
comprendre l'objectif de chaque répertoire : ::
    
    - app : contient tout ce qui est spécifique de votre application avec les
    javacripts (app/js/script.js) et les images (app/img)
    
    - css : contient les feuilles de style de base du framework

    - core : classe openMairie (version 4.2.0)
    
    - data : contient les requêtes sql permettant de créer votre application
            pgsql : pour postgresql
            mysql pour mysql
        dans les 2 cas, init.sql permet de créer les tables om_xx du framework
                        init_metier.sql sont les tables spécifiques de votre application
        d'autres scripts complètent la mise en œuvre des données notamment les
        modifications de base de données.
        
    - dyn : contient les fichiers de paramétrage du framework décrit ci dessous
    
    - gen : contient les scripts (obj) et requêtes (sql) générées par le générateur
    
    - img : contient les images du framework 
    
    - js : contient les javascripts de base du framework
    
    - lib : contient les librairies javascripts : openLayers et jquery
    
    - locales : contient les traductions de l'application
    
    - obj : contient les objets métiers surchargeant les objets générés (gen/obj)
    
    - pdf : contient les scripts d'édition du framework
    
    - php : contient les librairies php utilisées par le framework notamment : dbpear, phpmailer et fpdf
    
    - scr : contient les scripts d'affichage du framework
    
    - spg : contient les sous programmes génériques du framework
    
    - sql : contient les scripts sql surchargeant les scripts générés (gen/sql ...)
    
    - tmp : contient les fichiers temporaires créés par l'application
    
    - trs : contient les fichiers uploadés de l'application (par base /1 /2 ...)



Les répertoires à modifier pour une application sont les suivants :
    
    - app : vos scripts spécifiques
    - dyn : le paramétrage du framework et de l application
    - gen : les scripts php et sql générés
    - obj : vos objets métiers en surcharge
    - sql : les requêtes sql surchargées
    - tmp : les  fichiers temporaires
    - trs : les fichiers transférés en upload.
