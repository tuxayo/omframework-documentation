.. _modifier_base:

################
Modifier la base
################

Le framework openMairie permet de modifier la base , de regénérer
les scripts sans mettre en péril la personnalisation que vous avez effectuée


========================================
Rajouter un champ registre dans courrier
========================================

Il est proposer de rajouter un champ registre dans le courrier dont le but
est de stocker le numéro de registre du courrier sous la forme annee_numero_d_ordre

Nous allons d'abord créer un champ registre dans courrier de la manière suivante ::

    ALTER TABLE courrier ADD registre VARCHAR( 20 ) NOT NULL ;

Regénérer votre application courrier :

administration -> generateur -> courrier

laisser cochées les options par défaut :

    gen/obj/courrier.class.php
    
    gen/sql/mysql/courrier.inc.php
    
    gen/sql/mysql/courrier.form.inc.php
    
et valider.


Vous pouvez remarquer si vous allez sur le formulaire un nouveau champ registre
en fin de formulaire. Votre personnalisation n'est pas affectée.

Le numero de registre doit se mettre en ajout de manière automatique ,
une fois le formulaire validé:

Vous devez surcharger les méthodes suivantes dans obj/courrier.class.php ::

    
    // pour que registre ne soit pas modifiable
    function setType(&$form,$maj) {
        parent::setType(&$form,$maj);
         $form->setType('registre', 'hiddenstatic');
    }
    

    // avant l ajout de l enregistrement
    
    function triggerajouter($id,&$db,$val,$DEBUG)
    {
        //  prochain numero de registre
        //  fonction DB pear
        $temp= $db->nextId("registre");
        // fabrication du numero annee_no_d_ordre
        $temp= date('Y')."-".$temp;
        $this->valF['registre'] = $temp;
    }

Si vous souhaitez que registre apparaisse dans l'affichage de la table, vous
devez modifier le tableau champAffiche de sql/mysql/courrier.inc de la manière
suivante ::

    $champAffiche=array('courrier',
        'concat(substring(dateenvoi,9,2),\'/\',substring(dateenvoi,6,2),\'/\',substring(dateenvoi,1,4)) as dateenvoi',
        'concat(emetteur.nom,\' \',emetteur.prenom) as emetteur',
        'service.libelle as service',
        'registre');

Votre affichage de la table courrier est modifié.

   
================================
Rajouter l'adresse dans emetteur
================================

Il est proposé de rajouter l'adresse de l'emetteur :

- le libellé 

- le code postal

- la ville

Le script sql est le suivant ::

    ALTER TABLE emetteur ADD adresse VARCHAR( 40 ) NOT NULL ,
    ADD cp VARCHAR( 5 ) NOT NULL ,
    ADD ville VARCHAR( 40 ) NOT NULL ;


Regénérer votre application courrier :

administration -> generateur -> emetteur

laisser cochées les options par défaut :

    gen/obj/emetteur.class.php
    
    gen/sql/mysql/emetteur.inc.php
    
    gen/sql/mysql/emetteur.form.inc.php
    
et valider.

N'ayant pas modifié sql/mysql/emetteur.inc, vous n'avez aucune modification à faire


================================================
Améliorer la présentation du formulaire emetteur
================================================

Nous pouvons continuer à améliorer les présentations de nos formulaires 
en utilisant les méthodes setGroupe() et setRegroupe() dans le script
obj/emetteur.class.php

Il vous est proposé d'insérer dans votre script obj/emetteur.class.php
le code suivant ::

    function setGroupe(&$form, $maj) {
            
            $form->setGroupe('nom','D');
            $form->setGroupe('prenom','F');
            
            $form->setGroupe('cp','G');
            $form->setGroupe('ville','F');

    }

    function setRegroupe(&$form,$maj){
        $form->setRegroupe('nom','D', _('nom'), "collapsible");
        $form->setRegroupe('prenom','F','');
 
        $form->setRegroupe('adresse','D', ('adresse'), "startClosed");
        $form->setRegroupe('cp','G','');
        $form->setRegroupe('ville','F','');
    }


Le fieldset nom est affiché par défaut, pas celui de l'adresse.

Vos formulaires sont maintenant au point.

Le paragraphe suivant vous indique les surcharges d'openCourrier que vous
pouvez intégrés dans votre exemple.

Vous avez maintenant la méthode.

============================
les surcharges  openCourrier
============================

Vous pouvez vous reférez à openCourrier version 3.0.0 qui reprend une
base de données plus complexe.

C'est ainsi que courrier a deux sous formulaires : tache et dossier et qu'il
est aussi possible de compléter l'objet du courrier avec une bible.

Par contre les surcharges qui ont été faites dans cet exemple sont celles d'openCourrier.

Dans le script courrier.class.php d'openCourrier, il y a d'autre surcharge :

Les méthodes setLib, setGroupe et setRegroupe permettent **une présentation
en fieldset** (utilisation des champs vide1 à 5 voir sql/mysql/courrier.form.inc)

**La gestion des emetteurs enregistre dans la table courrier l'emetteur**:

- voir la methode setType qui utilise les combos et setSelect qui les paramétres

- voir la methode triggerAjouterapres qui enregistre l'emetteur saisi en courrier
dans la table emetteur si la case vide5 est cochée

Il est possible d'**afficher un courrier préalablement scanné** et d'
**enregistrer le fichier pdf dans dossier.class.php** après avoir écrit dessus
le numéro de registre

Voir les méthodes setType et triggerAjouterapres.

Dans dossier.class.php, vous avez un exemple de type upload pour télécharger des
fichiers.

L'objet obj/tachenonsolde.class.php est un **exemple de surcharge de tache.class.php**
qui affiche que les taches non soldées

openCourrier fonctionne avec des restrictions d'accès par service et les
méthodes de login ont été modifiées dans obj/utils.class.php ainsi qu'
utilisateur.class.php qui a dans openCourrier un champ service.


Vous pouvez aussi regarder deux scripts de traitement :

- trt/num_registre.php qui remet à 0 le numéro de registre

- trt/archivage.php qui tranfere en archive les courriers avant une date

*voir framework/util*