.. _presentation_generateur:

############
Présentation
############



**L'objectif est de construire une application sur la base de l'analyse des informations  du SGBD**


Les informations récupérées dans le SGBD sont les suivantes ::

    la liste des tables de la base de données

    les tables : nom, type , et longueur de chaque champs


Le générateur construit sur cette base le modèle de données sur les principes suivants: ::

     le nom de la clé primaire est le nom de la table et c'est le premier champ 
    
     la clé secondaire est le nom de la table en lien 
    
     si la clé est numérique, elle est automatique. 
    
     avec la multicollectivité, la création d'un champ « om_collectivite »
     met en place les accès multicollectivités d'openMairie 4



**Les assistants vont faciliter la mise en oeuvre des états**


Il est fourni avec le générateur un assistant pour faire les états et les sous états.



**openMairie est multi collectivité** : les états et les sous états sont générés dans la base de données et peuvent être associé a une collectivité.

Le générateur gére la multicollectivité si un champ « om_collectivité » est créé.

**Les schemas et prefixes sont gérés**
