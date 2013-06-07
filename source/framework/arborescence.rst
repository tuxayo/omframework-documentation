.. _arborescence:

############
Arborescence
############

Cette rubrique vise à décrire brièvement l'arborescence du framework pour
comprendre l'objectif de chaque répertoire. Elle est divisée en deux parties :
les répertoires spécifiques à l'applicatif qui sont modifiés lors du
développement de l'applicatif et les répertoires du framework qui sont récupérés
tel quel dans le framework.

``[F] .htaccess`` dans les descriptions de répertoires ci-dessous représente des
fichiers .htaccess empêchant l'accès dans les répertoires en question qui ne
doivent pas être accessibles depuis l'interface par l'utilisateur.


******************************************
Les répertoires spécifiques à l'applicatif
******************************************

Ces répertoires sont ceux qui font l'applicatif. Par exemple, sur un applicatif
comme openCimetière ce sont les répertoires qui vont se trouver dans le
gestionnaire de fichiers que l'on appelle les répertoires spécifiques.


``data/``
*********

Contient tous les fichiers d'initialisation de la base de données de
l'applicatif ::

    [D] data/
     |-[F] .htaccess
     |-[D] pgsql/
        |---- ...
     |---- ...


``app/``
********

Contient tout les scripts spécifiques à l'applicatif que ce soit des
javascripts ou des images ou des scripts PHP ::

    [D] app/ 
     |-[D] css/
        |---- ...
     |-[D] img/
        |---- ...
     |-[D] js/
        |---- ...
     |---- ...

``dyn/``
********

Contient les fichiers de paramétrage de l'applicatif ::

    [D] dyn
     |-[F] .htaccess
     |---- ...

``gen/``
********

Contient les scripts (obj) et requêtes (sql) générés par le générateur ::

    [D] gen
     |-[F] .htaccess
     |-[D] dyn/
        |---- ...
     |-[D] inc/
        |---- ...
     |-[D] obj/
        |---- ...
     |-[D] sql/
        |-[D] pgsql
        |---- ...
     |---- ...


``locales/``
************

Contient les fichiers de traduction de l'applicatif ::

    [D] locales/
     |-[F] .htaccess
     |---- ...


``obj/``
********

Contient les objets métiers surchargeant les objets générés ::

    [D] obj/
     |-[F] .htaccess
     |---- ...


``sql/``
********

Contient les scripts sql surchargeant les scripts générés ::

    [D] sql/
     |-[F] .htaccess
     |-[D] pgsql/
        |---- ...
     |---- ...


``tests/``
**********

Contient les jeux de tests unitaires et fonctionnels de l'applicatif ::

    [D] tests/
     |-[F] .htaccess
     |---- ...


``tmp/``
********

Contient les fichiers temporaires créés par l'applicatif ::

    [D] tmp/
     |-[F] .htaccess
     |---- ...


``trs/``
********

Contient les fichiers stockés par l'applicatif ::

    [D] trs/
     |-[F] .htaccess
     |-[D] 1/
        |---- ...
     |---- ...


****************************
Les répertoires du framework
****************************

Ces répertoires sont ceux qui sont issus du framework, c'est-à-dire qu'ils ne
sont pas dans l'applicatif lui même. Par exemple, sur un applicatif comme
openCimetière ce sont les répertoires qui vont être récupérés par une propriété
externals sur le gestionnaire de versions que l'on appelle les répertoires
du framework.

``php/``
********

Contient les librairies PHP utilisées par le framework (comme dbpear, phpmailer
ou fpdf) ::

    [D] php
     |-[F] .htaccess
     |---- ...


``core/``
*********

Contient les classes de la librairie du framework ::

    [D] core
     |-[F] .htaccess
     |---- ...


``css/``
********

Contient les feuilles de style de base du framework ::

    [D] css/
     |---- ...


``img/``
********

Contient les images du framework ::

    [D] img/
     |---- ...


``js/``
*******

Contient les javascripts de base du framework ::

    [D] js/
     |---- ...


``lib/``
********

Contient les librairies javascripts utilisées par le framework (comme openLayers
ou jquery) ::

    [D] lib/
     |---- ...


``pdf/``
********

Contient les scripts d'édition du framework ::

    [D] pdf/
     |---- ...


``scr/``
********

Contient les scripts d'affichage du framework ::

    [D] scr/
     |---- ...


``spg/``
********

Contient les sous programmes génériques du framework ::

    [D] spg/
     |---- ...

