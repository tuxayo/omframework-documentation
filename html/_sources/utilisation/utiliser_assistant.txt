.. _utiliser_assistant:

###############
Créer ses états
###############

Il vous est proposé de créer un état des courriers par service

Il sera utilisé dans ce chapître l'assistant état et sous état du générateur



====================
Créer l'état service
====================

Nous allons utiliser l'assistant état du générateur dans le menu :

administration -> générateur  : assistant

Choisir "créer un état"

Puis choisr dans le select, l'option service

Ensuite avec la touche "ctrl", sélectionner les champs service et libellé

Appuyer ensuite sur "import service dans la base"


.. image:: ../_static/utilisation_12.png


Un message apparait "service enregistré"

Vous avez créé un enregistrement qui a pour identifiant "service" dans
la table "om_etat".




Il faut maintenant permettre l'accès dans l'affichage du service.

Ouvrer le fichier sql/mysql/service.inc

Ajouter le script suivant ::

    $href[3] = array(
        "lien" => "../pdf/pdfetat.php?obj=".$obj."&amp;idx=",
        "id" => "",
        "lib" => "<img src=\"../img/pdf-16x16.png\" alt=\""
                 ._("Edition PDF")."\" title=\""._("Edition PDF")."\" />",
    );


Nous rajoutons la ligne 3 dans le tableau href. Vous avez un état lié
à l'affichage du service.


Il y a des exemples d'utilisation de href dans om_collectivité, om_etat,
om_utilisateur ...



===========================
Créer le sous état courrier
===========================


Nous allons utiliser l'assistant sous état du générateur dans le menu :

administration -> générateur  : assistant : sousetat

Nous choisissons la table courrier et nous surlignons les champs
dateenvoi, registre, objetcourrier et emetteur

Nous choisissons courrier.service comme clé secondaire pour faire le lien
avec service.


.. image:: ../_static/utilisation_13.png


En cliquant sur "import courrier dans la base", vous créez un enregistrement
ayant pour identifiant "courrier.service" dans la table om_sousetat

===================================================
Associer le sous état "courrier" à l'état "service"
===================================================

Vous devez rendre d'abord votre sous etat courrier.service actif pour pouvoir l'associer.

Allez dans l option du menu : administration -> sous etat

Recherchez le sous état "courrier.service" et modifier le en cochant sur actif
(1er fieldset)

Il vous faut maintenant associer le sous état "courrier.service" à l'état "service"

Allez dans l'option du menu administration -> etat.

Cherchez l'état "courrier" et modifiez le dans le fieldset (à déplier)
sous état selection, choisissez le sous état "courrier.service"

.. image:: ../_static/utilisation_14.png

Vous avez désormais un état des courriers par service :

.. image:: ../_static/utilisation_15.png


==========================================================
Mettre le nom et le prénom de l'emetteur dans le sous état
==========================================================

Nous souhaitons mettre le nom et le prénom de l'emetteur à la place de
la clé secondaire.

Vous devez modifier la requête sql de l'enregistrement courrier.service
dans la table om_sousetat de la manière suivante ::

    select  courrier.dateenvoi as dateenvoi,
            courrier.objetcourrier as objetcourrier,
            concat(emetteur.nom,' ',emetteur.prenom) as emetteur,
            courrier.registre as registre
            from &DB_PREFIXEcourrier inner join &DB_PREFIXEemetteur
            on emetteur.emetteur = courrier.emetteur
            where courrier.service='&idx'

Votre nouvel état a la forme suivante :

.. image:: ../_static/utilisation_16.png

Vous avez de nombreux exemples d'utilisation d'état et de sous état dans
les applications openMairie.

Une utilisation originale a été faite pour le cerfa du recensement dans
openRecensement où à la place du logo, il a été mis une image du cerfa.



On ne peut cependant pas faire tous les états et il est fort possible que vous ayez des
etats spécifiques. Vous avez des exemples d'utilisation spécifique des méthodes
de fpdf dans openElec : carte électorale, liste électorale ...



Vous pouvez compléter votre information avec le chapître *framework/edition*
et regarder les possibilités de paramétrage du générateur *generateur/parametrage*
pour la réalisation d'état customisé.
