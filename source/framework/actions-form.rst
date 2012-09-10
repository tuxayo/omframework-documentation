.. _actions-form:

############################
Les actions vers formulaires
############################

Les liens vers les formulaires sont principalement dans les tableaux et
formulaires de consultation d'objets.

Actions des tableaux
====================

La surcharge des actions de tableaux se fait via les scripts
sql/type_de_sgbd/nom_objet.inc.php (par defaut seules les actions d'ajout et
de consultation sont disponible).

L'ajout d'actions se présente de cette façon :

  .. code-block:: php

    <?php
        // Actions en coin ('corner') : ajouter
        $tab_actions['corner']['ajouter'] =
            array('lien' => 'form.php?obj='.$obj.'&amp;action=0',
                  'id' => '&amp;advs_id='.$advs_id.'&amp;tricol='.$tricol.
                          '&amp;valide='.$valide.'&amp;retour=tab',
                  'lib' => '<span class="om-icon om-icon-16 om-icon-fix add-16"
                              title="'._('Ajouter').'">'._('Ajouter').'</span>',
                  'rights' => array('list' => array($obj, $obj.'_ajouter'),
                                      'operator' => 'OR'),
                  'ordre' => 10,);

        // Actions à gauche ('left'): consulter
        $tab_actions['left']['consulter'] =
            array('lien' => 'form.php?obj='.$obj.'&amp;action=3'.'&amp;idx=',
                  'id' => '&amp;premier='.$premier.'&amp;advs_id='.$advs_id.
                          '&amp;recherche='.$recherche1.'&amp;tricol='.$tricol.
                          '&amp;selectioncol='.$selectioncol.'&amp;valide='.
                          $valide.'&amp;retour=tab',
                  'lib' => '<span class="om-icon om-icon-16 om-icon-fix
                            consult-16" title="'._('Consulter').'">'.
                            _('Consulter').'</span>',
                  'rights' => array('list' => array($obj, $obj.'_consulter'),
                              'operator' => 'OR'),
                  'ordre' => 10,);

        // Action sur la cinquième colonne de contenu
        $tab_actions['specific_content'][4] =
            array('lien' => 'form.php?obj='.$obj.'&amp;action=2'.'&amp;idx=',
                  'id' => '&amp;premier='.$premier.'&amp;advs_id='.$advs_id.
                          '&amp;recherche='.$recherche1.'&amp;tricol='.$tricol.
                          '&amp;selectioncol='.$selectioncol.
                          '&amp;valide='.$valide.'&amp;retour=tab',
                  'lib' => '<span class="om-icon om-icon-16 om-icon-fix
                            delete-16" title="'._('Consulter').'">'.
                            _('Consulter').'</span>',
                  'rights' => array('list' => array($obj, $obj.'_consulter'),
                                    'operator' => 'OR'),
                  'ordre' => 10,);

    ?>

Plusieurs emplacements d'actions existent :

- corner : actions dans la première cellule du tableau
- left : action situées dans la première colonne, disponibles pour chaque élément du tableau
- content : action sur le contenu du tableau
- specific_content : action sur une colonne de contenu du tableau

.. image:: ../_static/actions-form.png
   :height: 380
   :width: 800

Actions du menu contextuel de la consultation
=============================================

La configuration des actions du menu contextuel des formulaires en consultation
se fait via les scripts sql/type_de_sgbd/nom_objet.form.inc.php

Dans ces scripts, peuvent être surchargé, la liste des champs (ordre ou champs
affichés), requêtes sql permettant de remplir les widget de formulaires ainsi
.que les actions du menu contextuel.

L'ajout d'une action se presente de cette façon :

.. code-block:: php

   <?php
   $portlet_actions['edition'] = array(
       'lien' => '../pdf/pdflettretype.php?obj=om_utilisateur&amp;idx=',
       'id' => '',
       'lib' => '<span class="om-prev-icon om-icon-16 om-icon-fix pdf-16"
       					title="'._('Edition').'">'._('Edition').'</span>',
       'ajax' => false,
       'ordre' => 21,
   );
   ?>
