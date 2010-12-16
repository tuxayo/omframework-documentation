.. _reqmo:

#################
requete memorisee
#################

=========================
Description du parmetrage
=========================
 

Les parametres de la requete voie sont dans sql/sgbd/voie.reqmo.inc

    $reqmo['libelle'] contient le libéllé affiché en haut

    $reqmo['sql'] contient la requete SQL.
    
    Dans la requete, les paramétres sont mis entre []
    
    et ils sont définis en dessous  sous la forme requete[parametre]=:

        soit checked : la colonne est affiché ou non
    
        soit sous forme de tableau array(a,b) et le choix a ou b est donné à l utilisateur de requete
    
        soit avec un select : le choix se fait dans la table du select


La requete executée est celle qui est reconstituée avec les zones sasisies par l'utilisateur

Enfin, l utilisateur choisi soit un affichage
    
    soit en tableau,
    
    soit en csv avec un choix de séparateur.

(il n y a pas d'outill de fabrication de requête)

=======
Exemple
=======

voies sous openCimetiere ::


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

