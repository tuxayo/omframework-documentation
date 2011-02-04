.. _mapserver_mapfile:



Ce chapitre decrit les mapfiles

Documentation officielle : http://www.mapserver.org/mapfile/ 


Les mapfiles sont des fichiers texte qui vont contenir tous les paramètres nécessaires à 
MapServer pour la génération d'un document cartographique, statique ou dynamique.
Ils sont organisés en blocs (ou objets) hiérarchiques, imbriqués, notés NOM_DU_BLOC ... END. Le 
bloc racine est le bloc MAP


Généralités sur les files map
=============================


Non  sensibles à la casse (minuscules / majuscules), sauf

- pour les noms des champs attributaires, notés entre [crochets],

- et naturellement les valeurs de type chaîne, notées entre guillemets ou apostrophes. 
        

maximum 50 couches (layers) 
        
Les chemins peuvent être indiqués 
- de manière absolue (racine du système) 
- de maniere relative au mapfile : paramètre SHAPEPATH
(en l’absence de ce dernier même répertoire que le mapfile). 

=============
Le bloc MAP :
=============

la racine du mapfile, paramètres de la carte à générer. 
              
    Paramètres généraux ::
    
        NAME : nom de la carte, préfixe ajouté à tous les fichiers image générés 
        par le mapfile, donc à garder de courte taille, et entre guillemets.
        
        UNITS : Unité de la carte, utilisé pour la les calculs et le dessin de la barre 
        d'échelle. L'information sur l'unité du fond de carte n'est pas toujours 
        apportée par le bloc «  projection  », il faut donc essayer de la fournir 
        explicitement à chaque fois. 
                 Valeurs : [feet|inches|kilometers|meters|miles|dd] 
        
        EXTENT : Extension de la carte, donc coordonnées des extrémités de la 
        carte dessinée dans l'image générée par le mapfile. 
                 Valeurs : [xmin] [ymin] [xmax] [ymax] 
        
        STATUS : On ou Off, active ou désactive la totalité du mapfile. 
        
        FONTSET et SYMBOLSET : chemin et nom des fichiers contenant les 
        définitions de polices de caractère (utile sous linux) et de symboles (on peut 
        aussi définir un symbole directement dans le mapfile, mais c'est utile de 
        pouvoir les rassembler dans un seul fichier qui pourra être appelé par 
        différents mapfiles). 
        
        SHAPEPATH : Chemin vers le répertoire contenant les fichiers de données 
        (des couches), ou le chemin de base à partir duquel les paramètres DATA 
        iront les chercher. 
        
        ANGLE : Angle de rotation de l’image, en degrés. Utilisé en mode 
        MapScript, demande un bloc PROJECTION. 
        
        DEBUG : Active le débogage sur fichier de log et en précise le niveau. Une 
        variable système “MS_ERRORFILE” doit être présente et indiquer le chemin 
        du fichier de log. 
                    Valeurs : [off|on|0|1|2|3|4|5]
                    
    Paramètres de génération de l'image ::
        
        IMAGETYPE : type de fichier image à générer. Correspond à l'ensemble 
        inclut par défaut de blocs OUTPUTFORMAT. Ce paramètre dépend des 
        options avec lesquelles à été compilé MapServer. 
                 Valeurs : [gif|png|jpeg|wbmp|gtiff|swf|user_defined] 
        
        SIZE : Dimensions (largeur et hauteur) de l'image à générer, en pixels, 
        séparées par un espace. 
        
        RESOLUTION : Résolution de l'image en dpi, par défaut 72. N'affecte que 
        les calculs d'échelle, cf. le bloc SCALEBAR. 
        
        IMAGECOLOR : Couleur de fond de l’image de la carte (RGB).

    le bloc WEB paramétres du fonctionnement du serveur Web autour de la carte générée ::

        IMAGEPATH : Chemin absolu vers le répertoire temporaire où sera stocké 
        le fichier image généré par le mapfile. Doit se terminer par un slash « / », 
        
        IMAGEURL : Base de l'url (partie de l'adresse web s’ajoutant au nom du 
        serveur dans l'url) pointant sur le répertoire temporaire contenant les 
        fichiers image générés. Correspond à l'alias défini dans la configuration du 
        serveur Web.
        
        EMPTY : Url vers laquelle l'utilisateur est redirigé lorsqu'une requête 
        MapServer ne retourne pas de résultats.
        
        ERROR : Url vers laquelle l'utilisateur est redirigé lorsque se produit une 
        erreur. 
        
        LOG : Chemin et nom du fichier texte dans lequel sera inscrite l'activité de 
        MapServer sur ce mapfile. Contient une trace des opérations effectuées et 
        des éventuelles erreurs survenus. Fonctionne en parallèle avec le paramètre 
        
        DEBUG et les paramètres LOG des blocs LAYER du mapfile. 
        
        MINSCALE : Échelle minimale à laquelle le mapfile sera dessiné. Si une 
        échelle plus petite est demandée, MapServer dessinera la carte à l'échelle 
        précisée par ce paramètre. 
        
        MAXSCALE : Idem pour l'échelle maximale. 
        
        TEMPLATE : Chemin et nom du fichier modèle HTML éventuel (cf. seconde 
        partie du présent support de cours, dédiée à ce sujet). 
        
        HEADER : Modèle HTML à utiliser avant l'insertion de l'image de la carte. 
        
        FOOTER : Idem pour le bas de page. 
        
        METADATA : Bloc secondaire utiliser pour stocker des paires nom – valeur. 
        Utilisé par les modèles HTML (pour stocker des variables généralisées) et en 
        mode serveur WMS/WFS. 
        
    Le bloc REFERENCE ::
        
        Ce bloc définit les paramètres de la petite carte utilisée comme référence pour la carte 
        principale. C'est une image sur laquelle va être dessiné un rectangle représentant l'extension 
        de la carte principale, ou la localisation des résultats d'une requête, en mode QUERY. 
        Paradoxalement, MapServer a besoin d'une image fixe représentant la petite carte de 
        référence, il ne va pas la générer. Cela permet par contre l'utilisation d'une image externe. Il 
        faut donc alors, pour produire cette image, utiliser un mapfile simplifié (uniquement les 
        contours de la couche principale par exemple), réglé pour générer une image de petites 
        dimensions. MapServer va faire la relation entre la carte principale et la petite carte de 
        référence grâce aux paramètres EXTENT des deux cartes. 
        Par défaut le bloc REFERENCE a un paramètre STATUS à la valeur Off, pour activer cette 
        carte il faut donc penser à rajouter « STATUS ON » dans le bloc. 
        
    Le bloc LEGEND ::
    
        trois types de légendes : 
        • légendes simples sous forme d'images ; 
        • légendes basées sur un modèle de légende HTML (“template”, voir ci-après) ; 
        • légendes HTML pur. 
        Les légendes simples sont des images, inclues ou pas dans l'image de la carte 
        principale, reprenant chaque classe nommée des layers du mapfile et son figuré. Il faut donc 
        penser à nommer toutes les classes que l'on veut voir apparaître en légende (paramètre 
        NAME). Lorsque la légende simple est incluse dans l'image de la carte (paramètre : STATUS 
        EMBED), on peut préciser l'endroit où la légende sera dessinée avec le paramètre POSITION. 
        Ce paramètre prend une valeur correspondant à un code composé de deux lettres, la 
        première pour le haut / bas, u pour « upper », l pour « lower », la seconde pour gauche / 
        droite, l pour « left », r pour « right ». 
        Les caissons (rectangles) de légende pour les couches de polygones sont réglables en taille 
        avec le paramètre KEYSIZE (valeurs  : largeur, espace, hauteur) et leur espacement avec le 
        paramètre KEYSPACING (valeurs : écart horizontal espace écart vertical). Il n'est pas possible 
        de titrer une légende directement (tout comme pour la carte, on peut par contre le faire en 
        insérant la légende dans une page html ou utiliser un template html). 
            
    Le bloc PROJECTION ::
    
        voir mapserver_transform

    Le bloc SCALEBAR ::
    
        MapServer gère les échelles selon une technique assez particulière. En effet, il part du 
        principe que la carte sera au final une image possédant des dimensions en pixels, qui sera 
        visualisée au moyen d'un écran qui possède une certaine résolution. Par ailleurs, la carte doit 
        être dessinée dans un rectangle d'extension maximale donné par le paramètre EXTENT. 
        L'échelle finale de l'image doit donc être définie selon ces paramètres. Le paramètre EXTENT 
        prime sur le paramètre SCALE, car c'est lui qui définit plus précisément ce que doit contenir 
        la carte à dessiner.
        
        Pour dessiner une échelle indiquant une certaine longueur terrain, il faut donc déterminer 
        combien de pixels cette longueur va représenter (ou procéder à des essais), et l'indiquer dans 
        le paramètre SIZE du bloc.
        
    Le bloc SCALEBAR permet de dessiner des barres d'échelle, dans l'image de la carte ou 
    comme une image distincte. Il possède les paramètres suivants ::
    
         POSITION : Code à deux lettres définissant l'endroit où sera dessinée l'échelle, ce 
         code est le même que celui utilisé par le paramètre POSITION du bloc LEGEND, cf. 
         ci-dessus.
         
         SIZE : Dimensions en pixels (largeur espace hauteur) du rectangle contenant la barre 
         d'échelle. Important car détermine la longueur totale de la barre. 
         
         INTERVALS : Nombre de subdivisions à afficher. 
         
         STATUS : Inclusion (EMBED), dans une image à part (ON) ou annulation (OFF). 
         
         STYLE : Apparence de la barre, 0 donnant une barre de rectangles pleins de couleurs 
         alternées, 1 une barre fine munie de repères (barbules vers le haut). 
         UNITS : Unités pour le calcul de la longueur des intervalles et l’affichage de l’unité 
         de la barre d'échelle. Toutes unités possibles sauf degrés décimaux. 
         
         IMAGECOLOR : Couleur RGB du rectangle qui contient l'échelle. 
         
         BACKGROUNDCOLOR : Couleur RGB de la barre d'échelle et de ses libellés. 
         
         COLOR : Couleur alternative à BACKGROUNDCOLOR si barre de type 0 et plusieurs 
         intervalles spécifiés. 
         
         OUTLINECOLOR : Couleur RGB de la réserve autour de la barre d'échelle (mais pas 
         autour des libellés). 
         
         TRANPARENT : Valeur booléenne (ON / OFF) qui précise si le rectangle contenant 
         l'échelle est transparent. 
        
    Le bloc OUTPUTFORMAT 
        Ce bloc permet de définir précisément le format d'image du fichier qui sera généré par 
        MapServer. Le paramètre général IMAGETYPE correspond en fait à des blocs 
        OUTPUTFORMAT prédéfinis dans MapServer, par défaut. On peut ainsi mieux préciser 
        certains paramètres de la sortie image, par exemple le taux de compression pour le format 
        JPEG. 

        Exemple pour produire un JPEG en compression peu destructive : 
            OUTPUTFORMAT 
             NAME jpegfull 
             DRIVER "GD/JPEG" 
             MIMETYPE "image/jpeg" 
             IMAGEMODE RGB 
             EXTENSION "jpg" 
             QUALITY=100 
            END 
        Exemple pour utiliser la sortie avec anticrénelage AGG sur du PNG transparent : 
             OUTPUTFORMAT 
              NAME 'AGGA' 
              DRIVER AGG/PNG 
              IMAGEMODE RGBA 
             END
    
bloc LAYER  voir mapserver_layer
