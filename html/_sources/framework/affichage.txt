.. _affichage:

###################
Afficher les tables
###################

Il est décrit dans ce paragraphe l'affichage de requete sous forme de table
pour faire un choix d'ajout, de mise à jour ou de suppression.



.. image:: ../_static/tab_1.png


==========================
La requete SQL d'affichage
==========================

Elle se trouve dans sql/type_de_sgbd/nom_objet.inc

Les paramétres sont les suivants pour om_parametre.inc ::

    $serie=15;                                      Nombre d'enregistrement par page
    
    $ico="../img/ico_application.png";              Icone affiché
    
    $ent = _("option")." -> "._("om_parametre");    Titre du tableau
    
    $idz                                            affichage en haut du formulaire
    
    $table=DB_PREFIXE."om_parametre";               Table de référence
                                                    (il peut y avoir une ou plusieurs jointure)
    $champAffiche=array('om_parametre',
                        'libelle',
                        'valeur',
                        'om_collectivite');
    
    $champRecherche=array('libelle','valeur');      Champs pour la recherche
    
    $tri="";                                        Critere de tri par défaut
    
    $edition="om_parametre";                        edition pdf
    
    $sousformulaire= array()                        sous formulaire(s) associé(s)
    
    
    
    autre exemple de sous formulaire avec om_collectivite.inc
    
        $sousformulaire=array('om_etat',
                        'om_lettretype',
                        'om_parametre',
                        'om_sousetat',
                        'om_utilisateur');
                    
                    

====================
Le script scr/tab.php
====================

L'affichage se fait à partir du menu (voir *framework/parametrage*) sous la forme ::

    tab.php?obj=om_parametre
    
    où obj = nom_d_objet



=======================
Le composant openMairie
=======================

tab.php utilise les méthodes d'om_table.class.php qui est une classe d'openMairie ::

    php/openmairie/om_table.class.php
