####################
Convention de codage
####################

La convention de codage openMairie s'applique à tout le code qui fait partie 
de la distribution officielle d'openMairie ainsi qu'aux applicatifs de la gamme.
La convention de codage permet de conserver un code consistant et de le rendre 
lisible et maintenable facilement par les développeurs openMairie.


=====================
L'indentation du code
=====================

Pour améliorer la lisibilité, il faut utiliser une indentation de 4 espaces et
non pas des tabulations. En effet, les éditeurs de texte interprètent
différemment les tabulations alors que les espaces sont tous interprétés de la
même façon. De plus lors de commit, les historiques des gestionnaires de
versions (CVS ou SVN) sont faussés par ces caractères.

Il est recommandé que la longueur des lignes ne dépasse pas 75 à 85 caractères.


=====================
Encodage des fichiers
=====================

L'encodage des fichiers doit être UTF-8.


=====================
Tags dans le code PHP
=====================

Il faut utiliser toujours <?php ?> pour délimiter du code PHP, et non la version 
abrégée <? ?>. Cela est la méthode la plus portable pour inclure du code PHP 
sur des systèmes d'exploitations disposant de configurations différentes.


==================
HTML Valide et W3C
==================

Le Code HTML rendu doit être validé et correspondre aux standards W3C.


=============================
Les commentaires dans le code
=============================

Tous les fichiers PHP doivent avoir un entête de ce style ::

    <?php
    /**
     * Courte description du fichier
     *
     * Description plus détaillée du fichier (si besoin en est)...
     *
     * @package openmairie
     * @version SVN : $Id$
     */
    
    ?>

Quand et comment commenter son code ?
*************************************

L'objectif est de produire du code facilement lisible, qui permet à un dévelopeur débutant de comprendre la logique implémentée. Il faut donc éviter de paraphraser le code, et réserver les commentaires pour tout ce qui n'est pas compréhensible de premier abord, ou qui fait appel à de la logique *métier*.

Par exemple, éviter ce genre de commentaires ::

    // si $maj est plus grand que 3
    if ($maj >= 3) { 
        // alors on met $i à zéro
        $i = 0;
    }

... qui n'amène aucune information pertinente.

Le commentaire suivant, par contre, apporte une explication fonctionnelle pertinente ::

    // Dans le contexte du dossier d'autorisation alors le tableau affiche 
    // une colonne supplémentaire pour afficher le numéro du dossier
    if ($contexte == "dossier_autorisation") {
        $nb_col = 4;
    } else {
        $nb_col = 3;
    }

======
Images
======

Les fichiers images ajoutés dans les applications openMairie doivent être au
format PNG (Portable Network Graphics). Ce format permet d'obtenir des images
de qualité avec des propriétés de transparence.

