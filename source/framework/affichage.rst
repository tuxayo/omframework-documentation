.. _affichage:

###################
Afficher les tables
###################

Il est décrit dans ce paragraphe l'affichage de requete sous forme de table
pour faire un choix d'actions.


.. image:: ../_static/tab_1.png
   :height: 400
   :width: 800


=====================
Le script scr/tab.php
=====================

L'affichage se fait à partir du menu (:ref:`framework/parametrage<parametrage>`) sous la forme ::

    tab.php?obj=om_parametre
    
    où obj = nom_d_objet

==========================
La requete SQL d'affichage
==========================

Elle se trouve dans sql/type_de_sgbd/nom_objet.inc

Les paramétres sont les suivants pour om_parametre.inc.php

  .. code-block:: php

        <?php
            $serie=15;                                      //Nombre d'enregistrement par page
            $ico="../img/ico_application.png";              //Icone affiché (a voir deprecated)
            $ent = _("option")." -> "._("om_parametre");    //Titre du tableau
            $idz                                            //affichage en haut du formulaire
            $table=DB_PREFIXE."om_parametre";               //Table de référence (il peut y avoir une ou plusieurs jointure)
            $champAffiche=array('om_parametre',
                                'libelle',
                                'valeur',
                                'om_collectivite');
            $champRecherche=array('libelle','valeur');      //Champs pour la recherche
            $tri="";                                        //Critere de tri par défaut
            $edition="om_parametre";                        //edition pdf
            $sousformulaire= array()                        //sous formulaire(s) associé(s)

            //autre exemple de sous formulaire avec om_collectivite.inc.php
            $sousformulaire=array('om_etat',
                            'om_lettretype',
                            'om_parametre',
                            'om_sousetat',
                            'om_utilisateur');
        ?>
                    
                    

Ces script permettent aussi de configurer les actions disponibles sur le tableau (par defaut seules l'ajout et la consultation sont affichés).

Plusieurs emplacements d'actions existent :
    - corner : actions dans la première cellule du tableau
    - left : action situées dans la première colonne, disponibles pour chaque élément du tableau
    - content : action sur le contenu du tableau
    - specific_content : action sur une colonne de contenu du tableau

L'ajout d'actions se présente de cette façon :

  .. code-block:: php

    <?php
        // Actions en coin ('corner') : ajouter
        $tab_actions['corner']['ajouter'] =
            array('lien' => 'form.php?obj='.$obj.'&amp;action=0',
                  'id' => '&amp;advs_id='.$advs_id.'&amp;tricol='.$tricol.'&amp;valide='.$valide.'&amp;retour=tab',
                  'lib' => '<span class="om-icon om-icon-16 om-icon-fix add-16" title="'._('Ajouter').'">'._('Ajouter').'</span>',
                  'rights' => array('list' => array($obj, $obj.'_ajouter'), 'operator' => 'OR'),
                  'ordre' => 10,);

        // Actions à gauche ('left'): consulter
        $tab_actions['left']['consulter'] =
            array('lien' => 'form.php?obj='.$obj.'&amp;action=3'.'&amp;idx=',
                  'id' => '&amp;premier='.$premier.'&amp;advs_id='.$advs_id.'&amp;recherche='.$recherche1.'&amp;tricol='.$tricol.'&amp;selectioncol='.$selectioncol.'&amp;valide='.$valide.'&amp;retour=tab',
                  'lib' => '<span class="om-icon om-icon-16 om-icon-fix consult-16" title="'._('Consulter').'">'._('Consulter').'</span>',
                  'rights' => array('list' => array($obj, $obj.'_consulter'), 'operator' => 'OR'),
                  'ordre' => 10,);

        // Action sur la cinquième colonne de contenu
        $tab_actions['specific_content'][4] =
            array('lien' => 'form.php?obj='.$obj.'&amp;action=2'.'&amp;idx=',
                  'id' => '&amp;premier='.$premier.'&amp;advs_id='.$advs_id.'&amp;recherche='.$recherche1.'&amp;tricol='.$tricol.'&amp;selectioncol='.$selectioncol.'&amp;valide='.$valide.'&amp;retour=tab',
                  'lib' => '<span class="om-icon om-icon-16 om-icon-fix delete-16" title="'._('Consulter').'">'._('Consulter').'</span>',
                  'rights' => array('list' => array($obj, $obj.'_consulter'), 'operator' => 'OR'),
                  'ordre' => 10,);

    ?>



.. note::

    Il est possible d'ajouter plusieurs actions dans la première cellule du tableau.

.. note::

    Les surcharge de liens ajoutées depuis une version d'OpenMaire antérieur à la 4.3.0 sont pris en charge.

=======================
Le composant openMairie
=======================

tab.php utilise les méthodes d'om_table.class.php qui est une classe d'openMairie ::

    core/om_table.class.php

Les méthodes de ce composant peuvent être surchargées dans obj/om_table.class.php
