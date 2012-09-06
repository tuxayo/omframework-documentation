.. _interface:

===========
L'interface
===========


Le menu generateur est le suivant :


.. image:: ../_static/ecran_1.png

En appuyant sur la touche generation
on accède à l'écran de génération qui se décompose en :

- une analyse  de la base de donnée en cours et de la table choisie

- un état des fichiers existants ou non

- les options de génération


.. image:: ../_static/ecran_2.png

Analyse de la base
==================

Le programme propose une analyse de la base en cours :

- liste des tables de la base

- l'information sur la clé primaire de la table

- la longueur de l'enregistrement de la table

- les informations sur les champs : nom, type et longueur

- les clés secondaires (exemple table om_colllectivite)

- les sous formulaires à associer 


A partir de la version 4.2.0, il n y a plus de choix de paramétrage dans l'écran.

Les fichiers à générer
======================

Il est proposé une liste de case à cocher :

La case est cochée sur le fichier correspondant n'existe pas (colonne de droite)

Le formulaire métier auto généré, table.inc, tableform.inc est toujours coché (fichiers en gen/):

    gen/obj/table.class.php
    
    gen/sql/basededonnees/table.inc
    
    gen/sql/basededonnees/table.form.inc


La génération de ces 3 fichiers ne met pas en péril votre programmation qui est en :

    obj/table.class.php
    
    sql/basededonnees/table.inc
    
    sql/basededonnees/table.form.inc


basededonnees = mysql ou pgsql
