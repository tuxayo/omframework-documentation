.. _personnaliser:

#############################
Personnaliser son application
#############################

Nous allons maintenant personnaliser notre application

Pour se faire, nous allons saisir le jeu de données suivantes

Vous pouvez le faire avec les formulaires, la création des tables stockant les
sequences est fait par le framework (methode setId des objets metier) ::

    -- insertion de deux emetteurs
    
    INSERT INTO emetteur (emetteur, nom, prenom) VALUES
    (1, 'dupont', 'pierre'),
    (2, 'durant', 'jacques');
    
    --
    -- Structure de la table 'emetteur_seq'
    --
    
    CREATE TABLE emetteur_seq (
      id int(10) unsigned NOT NULL auto_increment,
      PRIMARY KEY  (id)
    ) TYPE=MyISAM ;
    
    --
    -- Contenu de la table 'emetteur_seq'
    --
    
    INSERT INTO emetteur_seq (id) VALUES (2);

    --
    -- Contenu de la table 'service'
    --
    
    INSERT INTO service (service, libelle) VALUES
    (1, 'informatique'),
    (2, 'telephonie');
    
    
    --
    -- Structure de la table 'service_seq'
    --
    
    CREATE TABLE service_seq (
      id int(10) unsigned NOT NULL auto_increment,
      PRIMARY KEY  (id)
    ) TYPE=MyISAM ;
    
    --
    -- Contenu de la table 'service_seq'
    --
    
    INSERT INTO service_seq (id) VALUES (2);

    --
    -- Contenu de la table 'courrier'
    --
    
    INSERT INTO courrier (courrier, dateenvoi, objetcourrier, emetteur, service) VALUES
    (1, '2010-12-01', 'Proposition de fourniture de service', 1, 1),
    (2, '2010-12-02', 'Envoi de devis pour formation openMairie', 2, 1);
    
    --
    -- Structure de la table 'service_seq'
    --
    
    CREATE TABLE service_seq (
      id int(10) unsigned NOT NULL auto_increment,
      PRIMARY KEY  (id)
    ) TYPE=MyISAM ;
    
    --
    -- Contenu de la table 'service_seq'
    --
    
    INSERT INTO service_seq (id) VALUES (2);


===================================
afficher un courrier plus convivial
===================================

L'affichage des courriers n'est pas lisible car il apparait les clés secondaires et non
les libellés.

Nous souhaitons avoir le nom_prenom de l'emetteur et le libellé du service.

Dans le fichier sql/mysql/courrier.inc nous allons modifier les variables  $table
et  $champAffiche de la manière suivante (après la ligne include)::

    $table = DB_PREFIXE."courrier  inner join ".DB_PREFIXE."emetteur
                    on emetteur.emetteur=courrier.emetteur
                    inner join ".DB_PREFIXE."service
                    on service.service=courrier.service";

    $champAffiche=array('courrier',
                    'concat(substring(dateenvoi,9,2),\'/\',substring(dateenvoi,6,2),
                                    \'/\',substring(dateenvoi,1,4)) as dateenvoi',
                    'concat(emetteur.nom,\' \',emetteur.prenom) as emetteur',
                    'service.libelle as service');


Le résultat est le suivant ::

    Courrier Dateenvoi  Emetteur  	    Service
        1 	01/12/2010 	dupont pierre 	informatique
        2 	02/12/2010 	durant jacques 	informatique

De la même manière nous souhaitons rechercher dans les courriers sur le
nom de l'emetteur et sur le libellé du service. Dans le fichier sql/mysql/courrier.inc,
nous allons modifier la variable tableau $champRecherche de la manière suivante ::

    $champRecherche=array("emetteur.nom", "service.libelle");
    
Vous devez avoir dans la zone recherche la possibilité de selectionner ::

    Tous
    emetteur.nom
    service.libelle


Nous souhaitons maintenant avoir les derniers courriers au début de la page affichée et nous
pouvons le faire en insérant la variable $tri dans courrier.inc de la manière suivante::

    $tri= " order by dateenvoi desc";

Le resultat est le suivant ::

        2  	02/12/2010  	durant jacques  	informatique
        1 	01/12/2010 	    dupont pierre 	    informatique


*Pour en savoir plus sur les variables framework/affichage*

=============================
Rendre obligatoire des champs
=============================

Nous avons affiché le courrier avec une jointure "inner".
Donc s'il n'y a pas de lien sur le service et/ou l'emetteur, l'enregistrement
n'apparaitra pas. Il faut rendre obligatoire la saisie de  l'emetteur et du service (auquel le courrier est affecté)

Nous allons surcharger la méthode verifier() dans obj/courrier.class.php de la manière suivante
(par défaut le premier champ, ici dateenvoi est obligatoire)

La methode est à inserer apres le constructeur est la suivante ::

    function verifier($val,&$db,$DEBUG) {
        parent::verifier($val,$db,$DEBUG);
        $f="&nbsp!&nbsp;&nbsp;&nbsp;&nbsp;";
        $imgv="<img src='../img/punaise.png' style='vertical-align:middle' hspace='2' border='0'>";
        if ($this->valF['service']==""){
            $this->msg= $this->msg.$imgv._('service')."&nbsp;"._('obligatoire').$f;
            $this->correct=False;
        }
        if ($this->valF['emetteur']==""){
            $this->msg= $this->msg.$imgv._('emetteur')."&nbsp;"._('obligatoire').$f;
            $this->correct=False;
        }
    }

La commande "parent::verifier($val,$db,$DEBUG);" permet de ne pas neutraliser la
fonction surchargée (ici dans gen/obj/courrier.class.php)

*Pour plus d'information voir framework/methode*

=============================
valoriser un champ par défaut
=============================

Pour simplifier la saisie, nous souhaitons mettre la date du jour dans le
champ dateenvoi.

Nous allons surcharger la methode setVal() dans obj/courrier.class.php
de la manière suivante ::

    function setVal(&$form, $maj, $validation, &$db, $DEBUG=null){
        parent::setVal($form, $maj, $validation, $db, $DEBUG=null);
        if ($validation==0) {
            if ($maj == 0){
                $form->setVal("dateenvoi", date('Y-m-d'));
            }
        }
    }

Le champ dateenvoi contient la date systeme (date('Y_m-d')) si la validation = 0 : premier affichage avant validation
 et $maj = 0 (on est en ajout)


============================
Mettre en majuscule un champ
============================

Nous souhaitons maintenant mettre en majuscule le champ "nom" de la table emetteur.
Nous allons surcharger la methode setOnchange() de l'objet emetteur dans
obj/emetteur.class.php de la manière suivante ::

    function setOnchange(&$form,$maj){
        parent::setOnchange($form,$maj);
        $form->setOnchange("nom","this.value=this.value.toUpperCase()");
    }

A la saisie ou à la modification du nom, le champ se met en majuscule.



========
Principe
========


Voila quelques exemples des possibilités de modification dans les fichiers sql
(repertoire sql/ ....) et dans les methodes de l'objet (repertoire obj/ ...)


En aucun cas, il ne faut modifier les fichiers dans gen/ qui est l'espace de travail du generateur,
**car cela permet de modifier la base et de pouvoir regenerer les écrans sans mettre en danger
votre personnalisation.**
