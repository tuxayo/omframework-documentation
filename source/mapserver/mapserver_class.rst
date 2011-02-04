.. _mapserver_class:



Ce chapitre decrit le bloc class, label et style contenu dans le bloc layer


##########
bloc class
##########

==========
bloc CLASS
==========

Ce bloc permet de définir des classes thématiques dans la couche, qui vont pouvoir 
être affichées différemment sur la carte globale.

Les blocs CLASS sont traités dans l'ordre du fichier map, selon l'ordre de classement vertical.

Les paramètres de ce bloc sont les suivants :

    NAME :  Nom de la classe,
            a préciser si l'on veut trouver cette classe dans la légende. 

    EXPRESSION : Critère de sélecton des objets de la couche qui vont être inclus 
            dans la classe en cours :
            comparaison de chaînes de caractères (en utilisant CLASSITEM),
            comparaisons logiques simples (avec opérateurs), 
            expressions régulières,
            fonction de chaîne length(). 

    COLOR : Couleur de fond des objets possédant une surface, exprimée en RGB, 

    OUTLINECOLOR : Couleur de contour des objets, en RGB aussi. 

    SYMBOL : Numéro ou nom du symbole, qui doit être défini dans le mapfile par un 
    bloc SYMBOL ou dans un fichier SYMBOLSET lié au mapfile. 

    SIZE : Taille du symbole ou de la trame, uniquement utilisable avec les symboles 
    redimensionnables. 

    MINSIZE et MAXSIZE : tailles mini et maxi (en pixels) de dessin des symboles. 
                        En dehors de cette fourchette ils sont dessinés à la valeur la plus proche. 

    SYMBOLESCALE :  Échelle à laquelle les symboles et/ou les textes apparaîssent à 
                    leur taille normale.
                    Obligatoire dans le cas de symboles proportionnels avec SIZEITEM. 

    TEXT : Nom de la colonne attributaire contenant le texte à utiliser pour 
    l'étiquetage des objets de la classe (On peut aussi créer des expressions combinant 
    plusieurs attributs entre crochets). 

    TEMPLATE : Chemin et nom du fichier modèle HTML éventuellement utilisé. 

    DEBUG : Valeur On ou Off. Si le paramètre LOGFILE est défini, les messages de 
    débogage détaillés seront ajoutés au fichier de log, en plus des messages d'erreur. 

Exemple de LABELITEM ::
    
	LABELITEM "zonelib"

	LABELMAXSCALE 5000
	LABELMINSCALE 1000

Les bloc CLASS peut contenir des blocs secondaires LABEL et STYLE.



===================
bloc LABEL ET STYLE
===================

Ce bloc permet de configurer l'étiquetage des éléments de la classe, selon un champ 
attributaire indiqué par le paramètre LABELITEM du bloc LAYER qui contient le bloc LABEL. 
                    
Paramètres ::
        
    TYPE : Type de police de caractère à utiliser.
        valeur «  bitmap  »,MapServer utilise ses propres polices internes,
                            pas de rotation, pas de dimensionement précis
        valeur «  truetype  », police vectorielle,FONT indique l'alias utilisé pou le fichier
                            tf dans le fichier FONTSET. 

    
    COLOR : Couleur du texte de l'étiquette, RGB.
    
    SIZE : Taille du texte,
        valeur la taille en pixels des polices TrueType, 
        valeur texte pour bitmap deMapServer, parmi [tiny|small|medium|large|giant].
    
    MINSIZE et MAXSIZE : Tailles mini est maxi  en pixels. 
    
    MINFEATURESIZE : Taille minimale (valeur entière en pixels) d'un objet à etiqueter
    .                La valeur « auto » étiquette que les objets au moins gros que l etiquette 
    
    ANTIALIAS : Réduction de « l'effet d'escalier ».
                Alourdit l'image produite car utilise des dégradés de couleurs
    
    OUTLINECOLOR : Couleur de la réserve autour du texte, en RGB. 
    
    OUTLINEWIDTH : Épaisseur de la réserve, en pixels. 
    
    SHADOWCOLOR et SHADOWSIZE : Couleur (RGB) et décalage (en pixels) pour le dessin des ombres. 
    
    BACKGROUNDCOLOR : Couleur du rectangle qui va contenir l'étiquette. 
    
    POSITION :  Valeur texte correspondant à la position de l'étiquette par rapport au 
                centre de l'objet qu'elle renseigne, selon le schéma suivant : 
                ul   uc    ur 
                cl   cc    cr 
                ll   lc    lr
                
            valeur « auto » test des 8 positions externes et choix de celle qui interfère le moins
                            avec les autres étiquettes de la classe. 
                
    ANGLE : Angle en degrés par rapport à la verticale, ou « auto » pour les objets 
            ligne, dans ce cas MapServer alignera l'étiquette sur l'objet. Auto ne fonctionne 
            que si les libellés utilisent une police truetype (vectorielle). 
                
    OFFSET : Valeur X Y de décalage entre le coin bas-droite de l'étiquette et le 
            centre de l'objet renseigné. 
                
    MINDISTANCE : Distance minimale (valeur entière en pixels) entre deux étiquettes du même objet. 
                
    BUFFER :Zone tampon (en pixels) autour des étiquettes, pour éviter qu'elles se touchent. 
                
    FORCE : Force le dessin des étiquettes de la classe, sans tenir compte des 
            contraintes de proximité. Valeur : true/false (à false par défaut). 
                
    PARTIALS :  Valeur true/false qui détermine si MapServer peut dessiner des 
                étiquettes incomplètes (coupées par les bords de l'image).
                Vaut false par défaut. 
        
    WRAP :  Précise le caractère (entre guillemets) du texte des étiquettes pour 
            indiquer un passage à la ligne et donc produire des libellés sur plusieurs lignes. 


Exemple de style couche cimetiere ::

	CLASS
        NAME "zone_style"
        STYLE
            ANGLE 45
            COLOR 64 70 101
            OUTLINECOLOR 255 255 255
            SIZE 15
            SYMBOL 0
            WIDTH 2
        END
        LABEL
            COLOR 0 192 0
            OUTLINECOLOR 255 255 255
        END	     
    END
