.. _framework:

############
presentation
############



=======================
le composant jquery
=======================

lib/


===
css
===

css/
feuilles de style cascadable

    main.css : principale openMairie
        specific.css : specifique a l application
            specific_om_... : suivant la feuille de style jquery choisi dans l application (1)
            
(1) voir parametrage config.inc.php


===============================
Mise en oeuvre dans les scripts
===============================

voir utilitaire

* facultatif : Description du role de la page

    $description = _("Cette page vous permet de .. ");
    $f->displayDescription($description);

* message d erreur
    $class : classe css qui s'affiche sur l'element
    ...
    $message = _("Mot de passe actuel incorrect");
    $f->displayMessage($class, $message);

* fieldset

    echo "<fieldset class=\"cadre ui-corner-all ui-widget-content\">\n";
    echo "\t<legend class=\"ui-corner-all ui-widget-content ui-state-active\">";
    echo _("Courrier")."</legend>";
        ...
    echo "</fieldset

ouvert

    echo "<fieldset class= ... collapsible\">\n";

ferme

    echo "<fieldset ... startClosed\">\n";

