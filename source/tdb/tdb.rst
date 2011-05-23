.. _tdb:

###############################
le tableau de bord paramétrable
###############################

ce paragraphe propose de décrire l'utilisation du tableau de bord paramétrable

========================
accès au tableau de bord
========================

Le paramétrage se fait en cliquant sur le lien "paramétrer son tableau de bord"

Il apparait alors ::

    un plus pour chaque colone pour ajouter un widget par colone
    une croix rouge pour supprimer un widget
    4 flèches pour déplacer les widgets


.. image:: ../_static/tdb_1.png


En cliquant sur "+", il est possible de rajouter des widgets dans son tableau de
bord

.. image:: ../_static/tdb_2.png

===============
la table om_tdb
===============


La table om_tbd comprend les champs suivants ::

    om_tdb int(8) NOT NULL,  : numero d ordre
    login varchar(40) NOT NULL, : login de l'utilisateur
    bloc varchar(10) NOT NULL, : bloc ou colone (c1 ou c2 ou c3)
    position int(8),   : position dans la colone
    om_widget int(8) NOT NULL, : numero de widget dans om_widget
    
