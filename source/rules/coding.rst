####################
Convention de codage
####################

La convention de codage openMairie s'applique à tout le code qui fait partie 
de la distribution officielle d'openMairie ainsi qu'aux applicatifs de la gamme.
La convention de codage permet de conserver un code consistant et de le rendre 
lisible et maintenable facilement par les développeurs openMairie.

.. contents::

=====================
L'indentation du code
=====================

Pour améliorer la lisibilité, il faut utiliser une indentation de 4 espaces et
non pas des tabulations. En effet, les éditeurs de texte interprètent
différemment les tabulations alors que les espaces sont tous interprétés de la
même façon. De plus lors de commit, les historiques des gestionnaires de
versions (CVS ou SVN) sont faussés par ces caractères.

============================
Longueur maximum d'une ligne
============================

Il est recommandé que la longueur des lignes ne dépasse pas 80 caractères afin de garder une bonne visibilité lors de l'affichage sur un terminal.

=====================
Encodage des fichiers
=====================

L'encodage des fichiers doit être UTF-8.


========
Tags PHP
========

Il faut utiliser toujours *<?php [...] ?>* pour délimiter du code PHP, et non la version 
abrégée *<? [...] ?>*. Cela est la méthode la plus portable pour inclure du code PHP 
sur des systèmes d'exploitations disposant de configurations différentes.
Un saut de ligne unique est ajouté après la fermeture de la balise.

Il faut éviter de mixer PHP et HTML. 

Préférer :

.. code-block:: php 
    
    <?php 
    var $plop = "attention";
    printf(
        '<script type="text/javascript">alert(\'%s\');</script>', 
        $plop
    );
    ?>

à :

.. code-block:: php 
    
    <?php 
    var $plop = "attention";
    ?>
    <script type="text/javascript">alert('<?php echo $plop; ?>');</script>


=====================
Règles typographiques
=====================

Accolades
*********

Les accolades sont asymétriques :

.. code-block:: php 

    <?php
    function ma_fonction() {
    }
    if ($variable === true) {
    }

Espacement
**********

Pour les opérateurs à deux composantes, l'opérateur est entouré d'espaces :

.. code-block:: php

    <?php
    $a = 2;
    $b = $c + 2;
    if ($a > 2) {
        $a = -1;   
    };
    $abc = "une" . "concaténation";
    
En cas de parenthèses :

    * insérez un espace avant dans les cas suivants :

        .. code-block:: php

            <?php
            if ($a == $b) {
                for ($a = 0; $a < $b, $a++) {
                    echo $a;
                }
            
            foreach ($a as $key => $value) {
                switch ($c) {
                    case 23: 
                        break;
                    case 4:
                        echo $b;
                }
            }

    * mais pas dans les cas suivants :
    
        .. code-block:: php

            <?php
            function azerty($a = null) {
                fopen($a);

Espace après une virgule :

    .. code-block:: php

        <?php
        function azerty($a = null, $b = "c") {

Indentation
***********

- tableaux :

    .. code-block:: php

        <?php
        $azerty = array(
            "a" => $b,
            "b" => $a,
        );

- fonctions et méthodes :

    * sur une ligne :

        .. code-block:: php

            <?php
            function azerty($a = null, $b = "c") {

    * sur plusieurs lignes (déprécié) :
    
        .. code-block:: php

            <?php
            public function copier_view(
                $enteteTab,
                $validation,
                $maj,
                &$db,
                $postVar,
                $aff,
                $DEBUG = false,
                $idx,
                $premier = 0,
                $recherche = "",
                $tricol = "",
                $idz = "",
                $selectioncol = "",
                $advs_id = "",
                $valide = "",
                $retourformulaire = "",
                $idxformulaire = "",
                $retour = "",
                $actions = array(),
                $extra_parameters = array()
            ) {

        .. note::
            
            Une fonction ou méthode ne devrait pas posséder autant de paramètres.

Nommage des classes, fonctions, méthodes et variables
*****************************************************

Les mots composant les noms de classes, fonctions, méthodes et variables doivent être séparés par un underscore et en minuscule (exemple : snake_case).

Le prototype des méthodes doit être identique à celui du parent (même nombre d'arguments, même typage).


Expressions
***********

Le php étant un language à typage faible il est nécessaire de comparer les retours de fonctions et méthodes de façon *stricte* :

.. code-block:: php

    <?php
    if (isset($a) === true) {
    }


==================
HTML Valide et W3C
==================

Le Code HTML rendu doit être valide selon les standards du W3C.


=============================
Les commentaires dans le code
=============================

Tout le code PHP doit être commenté selon les règles de PHPDocumentor https://www.phpdoc.org/docs/latest/index.html :

.. code-block:: php

    <?php
    /**
     * Courte description du fichier
     *
     * Description plus détaillée du fichier (si besoin en est)...
     *
     * @package openmairie
     * @version SVN : $Id$
     */
    
    (defined("PATH_OPENMAIRIE") ? "" : define("PATH_OPENMAIRIE", ""));
    require_once PATH_OPENMAIRIE."om_debug.inc.php";
    (defined("DEBUG") ? "" : define("DEBUG", PRODUCTION_MODE));
    require_once PATH_OPENMAIRIE."om_logger.class.php";

    /**
     * Définition de la classe edition.
     *
     * Cette classe gère le module 'Édition' du framework openMairie. Ce module
     * permet de gérer les différentes vues pour la génération des éditions PDF.
     */
    class edition {

        /**
         * Instance de la classe utils
         * @var resource
         */
        var $f = null;
    
        /**
         * Comparaison de chaines de caractères.
         * 
         * Fonction permettant de comparer les valeurs de l'attribut title
         * des deux tableaux passés en paramètre.
         * 
         * @param array $a Premier élément à comparer.
         * @param array $b Second élément à comparer.
         *
         * @return bool 
         */
        function sort_by_lower_title($a, $b) {
            if (strtolower($a["title"]) == strtolower($b["title"])) {
                return 0;
            }
            if (strtolower($a["title"]) < strtolower($b["title"])) {
                return -1;
            }
            return 1;
        }
    
    }
    
    ?>

Quand et comment commenter son code ?
*************************************

L'objectif est de produire du code facilement lisible, qui permet à un dévelopeur débutant de comprendre la logique implémentée. Il faut donc éviter de paraphraser le code, et réserver les commentaires pour tout ce qui n'est pas compréhensible de premier abord, ou qui fait appel à de la logique *métier*.

Par exemple, éviter ce genre de commentaire :

.. code-block:: php

    <?php
    // si $maj est plus grand que 3
    if ($maj >= 3) { 
        // alors on met $i à zéro
        $i = 0;
    }

... qui n'amène aucune information.

Le commentaire suivant, par contre, apporte une explication fonctionnelle pertinente :

.. code-block:: php

    <?php
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

