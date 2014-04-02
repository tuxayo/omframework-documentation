.. _import:

###########################
Importer des données en csv
###########################

Il est possible d'importer des données suivant des scripts pré paramétrées mais qui sont
modifiables.

Pour lancer le menu import, prenez l'option : administration -> import

import_script.php permet les imports dans la base de donnees de fichier au
format csv telecharge :

Exemple de format de fichier à importer (utilisateur.txt)::

    nom,login,pwd,profil
    "Georges DANDIN";"Georges";"21232f297a57a5a743894a0e4a801fc3";"3"
    "Raymond DAVOS";"Raymond";"fe01ce2a7fbac8fafaed7c982a04e229";"3"
    "Albert DUPONT";"Albert";"05c7e24700502a079cdd88012b5a76d3";"6"


La description du transfert se fait dans le fichier extension import_nomobjet.inc dans /sql/...:

exemple : import_script.php?obj=utilisateur

dans utilisateur.import .inc , il est defini: ::

    
    le message affiché en import :
        $import= "Insert utilisateur" : Message
    
    la table d importation
        $table= "utilisateur"
        
    le clé primaire si elle est automatique (mise en place d une séquence)
    ce champ est vide sinon 
        $id="utilisateur"
        
    Le verrouillage de la base de sonnées
        $verrou = 1  mise a jour de la base
                = 0 pas de mise a jour pour une phase de test
                
    Le mode debug
        $DEBUG  =1 affichage des enregistrements a l ecran
                =0 pas d affichage
                
    La mise en place d un fichier d'erreur :
        $fic_erreur=1 fichier erreur
                   =0 pas de fichier d erreur

    La mise en place d un fichier de rejet reprenant les enregistrements csv rejetés
    ce fichier contient les enregistrements en erreur et permet de relancer le
    traitement apres correction (manuelle)
        $fic_rejet=1 fichier de rejet pour relance traitement
                  =0 pas de fichier rejet

    La première ligne affiche le nom des champs :
    $ligne1=1 la premiere ligne contient les noms de champs
           =0 sinon
    
    Les zones obligatoires : tableau $obligatoire
    
        $obligatoire['nom']=1;// obligatoire = 1
        $obligatoire['login']=1;// obligatoire = 1
    
    les tests d'existence d'une clé secondaire
    
        $exist['profil']=1 => 0=non / 1=oui
        $sql_exist["profil"]= "select profil from profil where profil = '"
    
    La liste des champs à insérer
    il faut mettre en commentaire les zones non traitées
        $zone['nom']='0' => la 1ère zone contient le nom
        $zone['login']=1 => la 2éme zone contient le login
        $zone['pwd']='2' => la 3éme zone contient le mot de passe (crypte)
        $zone['profil']='3' => la 4éme zone contient le profil
    
    La valeur par defaut :
    En effet, si $zone['profil']="" on peut definir un profil par defaut
        $defaut['profil']='5' Le profil par defaut  sera 5 