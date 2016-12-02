.. _layout:

###################################
Abstraction du 'layout' (ergonomie)
###################################

.. warning::

   Cette rubrique est en cours de rédaction.

.. contents::

Depuis la version openMairie 4, il est utilisé l'ergonomie de jquery.

===================
Le composant jquery
===================

Les skins jquery peuvent être rajoutés dans le répertoire /om_theme/.

Il est possible de générer un nouveau thème directement depuis le site
de Jquery-UI ::

    http://jqueryui.com/download    pour télécharger un thème standard
    http://jqueryui.com/themeroller pour créer un thème personnalisé

Le changement de thème peut se faire dans le fichier EXTERNALS.txt

*voir framework/paramétrage*

=====================
Les feuilles de style
=====================

Les feuilles de style sont stockées dans le repertoire css/ et sont
cascadables ::

    main.css        : feuille de style principale openMairie
    om-theme/om.css : suivant la feuille de style jquery (voir EXTERNALS.txt)
    app/css         : surcharge spécifique a l'application (exemple : le logo de
                      l'application)
