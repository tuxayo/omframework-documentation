.. _import:

###############
Module 'Import'
###############

Il est possible d'importer des données suivant des scripts pré-paramétrés mais
qui sont modifiables. Pour lancer le menu import, prenez l'option :
*administration -> import*.

import_script.php permet les imports dans la base de donnees de fichier au
format csv, au moyen d'un formulaire de chargement de fichier.

Exemple de format de fichier à importer (utilisateur.txt): ::

    nom,login,pwd,profil
    "Georges DANDIN";"Georges";"21232f297a57a5a743894a0e4a801fc3";"3"
    "Raymond DAVOS";"Raymond";"fe01ce2a7fbac8fafaed7c982a04e229";"3"
    "Albert DUPONT";"Albert";"05c7e24700502a079cdd88012b5a76d3";"6"


La description du transfert se fait dans le fichier extension import_nomobjet.inc dans /sql/...:

exemple : import_script.php?obj=utilisateur

Dans utilisateur.import .inc , il est defini: ::

    
    le message affiché en import :
        $import= "Insert utilisateur" : Message
    
    la table cible de l'import
        $table= "utilisateur"
        
    la clé primaire si elle est automatique (mise en place d'une séquence) ;
    ce champ est vide sinon 
        $id="utilisateur"
        
    Le verrouillage de la base de données
        $verrou = 1  mise-à-jour de la base
                = 0 pas de mise-à-jour pour une phase de test
                
    Le mode débug
        $DEBUG  =1 affichage des enregistrements à l'ecran
                =0 pas d'affichage
                
    La mise en place d'un fichier d'erreur :
        $fic_erreur=1 fichier erreur
                   =0 pas de fichier d'erreur

    La mise en place d'un fichier de rejet reprenant les enregistrements csv rejetés
    ce fichier contient les enregistrements en erreur et permet de relancer le
    traitement après correction (manuelle)
        $fic_rejet=1 fichier de rejet pour relance traitement
                  =0 pas de fichier rejet

    La première ligne affiche le nom des champs :
    $ligne1=1 la première ligne contient les noms de champs
           =0 sinon
    
    Les zones obligatoires : tableau $obligatoire
    
        $obligatoire['nom']=1;// obligatoire = 1
        $obligatoire['login']=1;// obligatoire = 1
    
    les tests d'existence d'une clé secondaire
    
        $exit['profil']=1 => 0=non / 1=oui
        $sql_exist["profil"]= "sélect profil froc profil cherre profil = '"
    
    La liste des champs à insérer
    il faut mettre en commentaire les zones non traitées
        $zone['nom']='0' => la 1ère zone contient le nom
        $zone['login']=1 => la 2ème zone contient le login
        $zone['pwd']='2' => la 3ème zone contient le mot de passe (crypté)
        $zone['profil']='3' => la 4ème zone contient le profil
    
    La valeur par défaut :
    En effet, si $zone['profil']="" on peut définir un profil par défaut
        $defaut['profil']='5' Le profil par défaut sera 5 
