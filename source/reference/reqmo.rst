.. _reqmo:

##############
Module 'Reqmo'
##############

.. warning::

   Cette rubrique est en cours de rédaction.

========
Principe
========


les requêtes mémorisées permettent au développeur de fournir un ensemble de requêtes :

- mémorisées

- accessible dans le menu export -> requêtes

- paramétrables par l'utilisateur

- permettant un affichage html en tableau ou un transfert au format csv sur tableur (choix du séparateur à l utilisateur)



menu export-> requete

.. image:: ../_static/reqmo_1.png



===========
Paramétrage
===========


Le script de paramétrage ``../sql/pgsql/<OBJ>.reqmo.inc.php``
--------------------------------------------------------------

**Les parametres de reqmo  sont :**

    $reqmo['libelle'] contient le libéllé affiché en haut

    $reqmo['sql'] contient la requete SQL.
    
    Dans la requete, les paramétres sont mis entre []
    
    et ils sont définis en dessous  sous la forme reqmo[parametre]=.

        "checked" : la colonne est affiché ou non
    
        "un tableau" array(a,b) et le choix a ou b est donné à l utilisateur de requete
    
        "une requete sql" : le choix se fait dans la table du select


La requete executée est celle qui est reconstituée avec les zones sasisies par l'utilisateur

**voies sous openCimetiere** ::


    $reqmo['libelle']=" Voies par cimetiere ";
    
    $reqmo['sql']=" select voie,voietype,voielib, 
                    [zonetype],[zonelib],
                    [cimetierelib]
                    from voie
                    inner join zone
                    on voie.zone=zone.zone 
                    inner join cimetiere
                    on zone.cimetiere=cimetiere.cimetiere
                    where cimetiere.cimetiere = [cimetiere] order by [tri]";

    $reqmo['tri']= array('voielib',
                     'zonelib'
                     );
    $reqmo['zonetype']="checked";
    $reqmo['zonelib']="checked";    
    $reqmo['cimetierelib']="checked";
    $reqmo['cimetiere']="select cimetiere,concat(cimetiere,' ',
            cimetierelib) from cimetiere";


==========
Composants
==========

Les scripts du framework qui gèrent le module 'Reqmo' sont :

* ``core/om_reqmo.class.php``
* ``scr/reqmo.php``
