.. _ergonomie:

##########
L'ergonomie
##########

Depuis la version openMairie 4, il est décidé d'utiliser l'ergonomie de jquery.



===================
Le composant jquery
===================

Les skins jquery peuvent être rajoutés dans le repertoire /lib/jquery_ui.

Le changement de skin se fait dans le fichier dyn/config.inc.php

*voir framework/parametrage*

=====================
Les feuilles de style
=====================

Les feuilles de style sont stockées dans le repertoire css/ et sont cascadables ::

    main.css : principale openMairie
        specific.css : specifique a l application
            specific_om_... : suivant la feuille de style jquery choisi dans l application (1)
   


==================================
La mise en oeuvre dans les scripts
==================================

*voir framework/utilitaire*



