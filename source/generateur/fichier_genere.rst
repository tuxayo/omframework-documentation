.. _fichier_genere:

####################
Les fichiers générés
####################

Les fichiers générés concernent :

- les formulaires

- les requêtes mémorisées

- le script d'import de données

***************
Les formulaires
***************

Les formulaires sont génèrés suivant le nom de la table dans le répertoire sql, sous repertoire portant le nom de la base pour régler le problème de compatibilité SQL (concaténation, extraction ...) 

Deux types de formulaire sont générés : type table, type form.

--------------------------
Paramétres de type table :
--------------------------

- gen/sql/basededonnees/nom_table.inc

- sql/basededonnees/nom_table.inc


Par défaut :

- tri en affichage vide

- champ de recherche avec les champs string

- pas d'affichage de champ blog

- rattachement de sous formulaire

- affichage de l'édition de la table


Dans le fichier paramètres : form.inc

$serie = nombre d'enregistrement par page

$ico = icône par defaut

-------------------------
Paramétres de type Form : 
-------------------------

gen/sql/basededonnees/nom_table.form.inc

sql/basededonnees/nom_table.form.inc

Dans le fichier paramètres : form.inc

$ico = icône par defaut

Par défaut :
- tous les champs sont affichés les uns en dessous des autres

*********************
Les Objets « métier »
*********************

L'objet métier généré est stocké en gen/obj/nom_table.class.php. Ce script ne doit pas être modifié car il est reconstitué à chaque génération :

Cela permet de pouvoir modifier la base de données (ajout, modification ou suppression de champs) et de regénérer tout ou partie de l'application

Un second script héritant de l'objet généré permet de surcharger les méthodes et de personnalisé l'objet métier.

Toutes les modifications doivent être faites dans ce script soit en héritant de la méthode,
soit en surchargeant la méthode.


L'objet à personnaliser est stocké en obj/nom_table.class.php

Les méthodes  générés dans l'objet métier gen/obj/nom_table.class.php sont par défaut les suivantes. 

Le type de champs est :

  
    . caché (hidden) en ajout pour la clé primaire automatique, 
 
    . modifiable en ajout si la clé primaire n'est pas automatique
    
    . l'unicité de la clé primaire est vérifiée si elle est modifiable (version 4.2.0)
 
    . la clé primaire est visible sans possibilité de modifier en modification
 
    . la clé secondaire n'est pas modifiable en sous formulaire si c'est la clé primaire du formulaire
 
    . la clé secondaire est un champ select qui reprend les informations de la table liée
 
    . la date est au format français
    
    . geom si ce champ est géométrique (version 4.2.0)  


La longueur d'affichage et le maximum autorisé à la saisie est celle contenu dans la base d'origine

Le contrôle des clés secondaires des autres tables est généré : il n'est pas possible de supprimer un enregistrement si des enregistrements sont liés à la clé primaire

Il est vérifier l'unicité de la clé si elle n'est pas automatique (version 4.2.0)

Les libellés sont les noms des champs.



Ce module sert pour le formulaire et le(s) sous formulaire(s).

Les méthodes qui peuvent être implémentés dans obj/nom_table.class.php sont les suivantes ::

    - verifier   
    - regroupe et groupe pour modifier les présentations
    - trigger avant ou après l'enregistrement:
    - triggerajouter
    - triggermodifier
    - triggersupprimer
    - triggerajouterapres
    - triggermodifierapres
    - triggersupprimerapres


Les méthodes de l'objet généré en gen/obj  peuvent être surchargées totalement ou partiellement :

Exemple ::
    
    om_profil.class.php :
        surcharge des méthodes
            setValFAjout setId,
            verifierAjout
            et setType car la clé primaire est numérique et non automatique
    
    om_utilisateur.class.php :
        champ pwd pour mot de passe  methode partiellement surchargées (parent::setvalF($val);) setvalF, setType, setValsousformulare, surcharge avec un javascript de mise en majuscule du nom


Enfin, il est possible de mettre en place d'autres type de champs disponible dans openMairie ::

    - ComboG  combo gauche
    - comboD combo droit   
    - Localisation (geolocalisation en x, y)
    - http (lien)
    - httpclick (lien)
    - Password (Mot de passe)
    - Pagehtml (Textearea pour affichage html)
    - Textdisabled (Text non modifiable)
    - Selectdisabled (Select non modifiable)
    - Textreadonly (Text non modifiable)
    - Hidden (champ caché)
    - Checkbox (case a cocher oui/non)
    - Upload (chargement d'un fichier)
    - voir (voir un fichier téléchargé)
    - Rvb (choisir une couleur rvn avec la Palette de couleur) ...

voir framework/formulaire



*********
Les états
*********

Seul l'état « pdf » est généré par le générateur 

Dans le menu gen (generateur), les états sont générés automatiquement avec un assistant.

Cet assistant vous permet de construire un état :

- en choisissant une table de la base

- en choisissant les champs à mettre dans l'état

L'etat est enregistré dans la table om_etat et peut être modifié
menu->administration -> etat

De la même manière, il est possible de créer un sous etat.

Il est possible de choisir le champ qui sera la clé secondaire en lien avec la table mère

Le sousetat est enregistré dans la table om_sousetat et peut être modifié

menu->administration -> sousetat


Le calcul de la largeur des colonnes est automatique dans les sous états et l'état pdf.

Attention :  les champs « blob » ne sont pas pris en compte dans les éditions.

***********************
les requêtes mémorisées
***********************

Les requêtes paramétrées sont crées suivant le principe suivant :

    - une requête globale
    
    - une requête avec un champ select pour chaque clé secondaire (il est possible de sélectionner la requête à générer)
    
    - Les autres champs sont sélectionnés à l'affichage

Les requêtes sont accessibles dans l'option du menu -> export.


***********
les imports
***********

Un script d'import des données est généré suivant le principe suivant :

    - si la clé est automatique, génération du compteur
    
    - tous les champs sont importés
    
    - vérification de l'existence de la clé secondaire à chaque enregistrement 


Les tables avec clés secondaires doivent donc être importées en dernier.

   
