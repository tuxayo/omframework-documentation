.. _modifier_base:

##############################
Modifier la base et re générer
##############################

Le framework openMairie permet de modifier la base , et de prendre en
compte ces modifications en regénérant les scripts sans mettre en péril
la personnalisation que vous avez effectuée

Nous vous proposons de rajouter un champ registre dans la table courrier
et de rajouter l'adresse dans la table emetteur.


========================================
Rajouter un champ registre dans courrier
========================================

Il est proposer de rajouter un champ registre dans le courrier dont le but
est de stocker le numéro de registre du courrier sous la forme annee_numero_d_ordre

Nous allons d'abord créer un champ registre dans courrier de la manière suivante ::

    ALTER TABLE courrier ADD registre VARCHAR( 20 ) NOT NULL ;

Vous devez regénérer votre application courrier dans l'option du menu : administration -> generateur -> courrier
et laisser cochées les options par défaut :

    gen/obj/courrier.class.php
    
    gen/sql/mysql/courrier.inc.php
    
    gen/sql/mysql/courrier.form.inc.php
    

Validez l'opération.


Vous pouvez remarquer si vous allez sur le formulaire un nouveau champ registre
en fin de formulaire. Votre personnalisation n'est pas affectée.

Nous voulons que le numero de registre se mette en ajout de manière automatique ,
une fois le formulaire validé:

Il faut donc surcharger les méthodes suivantes dans obj/courrier.class.php ::

    
    // pour que registre ne soit pas modifiable
    function setType(&$form,$maj) {
        parent::setType(&$form,$maj);
         $form->setType('registre', 'hiddenstatic');
    }
    

    // pour la mise a jour de la séquence avant l ajout de l enregistrement
    
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
devez aussi modifier le tableau champAffiche de sql/mysql/courrier.inc de la manière
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

Il est proposé de rajouter l'adresse de l'emetteur à savoir : le libellé, le code postal et
la ville. Le script sql est le suivant ::

    ALTER TABLE emetteur ADD adresse VARCHAR( 40 ) NOT NULL ,
    ADD cp VARCHAR( 5 ) NOT NULL ,
    ADD ville VARCHAR( 40 ) NOT NULL ;


Vous devez regénérer votre application courrier en allant dans l'option du menu :
administration -> generateur -> emetteur et laisser cochées les options par défaut :

    gen/obj/emetteur.class.php
    
    gen/sql/mysql/emetteur.inc.php
    
    gen/sql/mysql/emetteur.form.inc.php
    

Validez l'opération.

N'ayant pas modifié sql/mysql/emetteur.inc, le framework fonctionne avec le code généré



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
        
        $form->setGroupe('cp','D');
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
pouvez intégrer dans votre exemple, maintenant que vous avez la a méthode.


=============================
Les surcharges d'openCourrier
=============================

Vous pouvez utiliser openCourrier version 3.0.0 qui est téléchargeable au lien suivant :

http://adullact.net/frs/?group_id=297


La base de données d'openCourrier est plus complexe. C'est ainsi que
courrier a deux sous formulaires : tache et dossier et qu'il
est aussi possible de compléter l'objet du courrier avec une bible.

Si les surcharges qui ont été faites dans notre exemple sont celles d'openCourrier, il y a
d'autre surcharge dans le script courrier.class.php d'openCourrier,  :

Les méthodes setLib, setGroupe et setRegroupe permettent **une présentation
en fieldset**  du courrier (utilisation des champs vide1 à 5 voir sql/mysql/courrier.form.inc)

**La gestion des emetteurs enregistre dans la table courrier l'emetteur** (voir la méthode
setType qui utilise les combos, la méthode setSelect qui les paramétre
et la méthode triggerAjouterapres qui enregistre l'emetteur saisi en formulaire courrier
dans la table emetteur si la case vide5 est cochée)

Il est possible d'**afficher un courrier préalablement scanné** et d'
**enregistrer le fichier pdf dans dossier.class.php** après avoir écrit dessus
le numéro de registre (Voir les méthodes setType et triggerAjouterapres).

Il y a d'autres objet métier qui ont des surcharges intéressantes :

Dans dossier.class.php, vous avez un exemple de type upload pour télécharger des
fichiers.

L'objet obj/tachenonsolde.class.php est un **exemple de surcharge de tache.class.php**
qui affiche que les tâches non soldées

openCourrier fonctionne avec des restrictions d'accès par service et **les
méthodes de login** ont été modifiées dans obj/utils.class.php ainsi qu'
utilisateur.class.php qui a dans openCourrier un champ service.


Vous pouvez aussi regarder **deux scripts de traitement** :

- trt/num_registre.php qui remet à 0 le numéro de registre

- trt/archivage.php qui tranfere en archive les courriers avant une date

Vous avez plus de détail sur les traitements dans le chapître
*framework/util* notament sur la mise à jour du registre.
