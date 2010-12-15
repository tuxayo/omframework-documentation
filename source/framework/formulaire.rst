.. _parametrage:

##########
Formulaire
##########


 

 
methodes

La classe formulaire.class.php a les méthodes suivantes :

Les methodes sur les controles du formulaire :

- Hiddenstatic -> Champ non modifiable  Valeur récupéré par le formulaire
- Hiddenstaticdate -> date non modifiable Valeur récupéré par le formulaire
- statiq -> Valeur non modifiable
- date -> date modifiable  js/calendrier
- select -> Controle select
- selectdisabled -> Controle select non modifiable
- Text -> Controle text
- hidden -> Controle non visible avec valeur conservée
- password -> Controle password
- Textdisabled -> Controle text non modifiable
- comboG -> Appel à un programme de correspondance à une table
            Cas ou il y a une grosse table en correspondance
            spg/combo
- ComboD -> Appel à un programme de correspondance à une table
            Cas ou il y a une grosse table en correspondance
            spg/combo
- Upload -> spg/upload et spg/voir.php
- textarea
- textarea_html
- localisation -> spg/localisation.php
- rvb -> spg/rvb.php
 
Les  methodes de construction et d affichage

- afficher() affichage des champs (appelle par dbformdyn.class.php : methode formulaire
           -> afficherChampRegroupe() affichage des champs par regroupement / groupement
           -> afficherChamp() affichage de champ sans regroupe
- recupererPostvarsousform() et recuperePostVar() recuperation des variables apres validation
- enpied() presentation

Les methodes assesseurs qui change les valeurs des proprietes de formulairedyn
- setType()
- setVal()
- setLib()
- setSelect()
- setTaille()
- setMax()
- setOnchange()
- setKeyup()
- setOnclick()
- setSelect()
- setGroupe()
 D premier champ du groupe
 G champ groupe
 F dernier champ du groupe
- setRegroupe()
 D premier champ du fieldset
 G champ dans le fieldset
 F dernier champ du fieldset

 
et enfin les methodes de date :
- dateAff($val)

========================
Sous programme generique
========================

- js/script.js

appel au script d affichage des sous programme generique

- spg/combo.php
  parametrage dans
       dyn/comboparametre.inc.php
       dyn/comboretour.inc.php
       dyn/comboaffichage.inc.php

- spg/localisation.php et js/localisation.js

- spg/voir.php 
- spg/upload.php
        php/openmairie/upload.class.php (composant openMairie)
        parametrage de fichier autorise dans dyn/config.inc.php

- spg/rvb.php et js/rvb.js

