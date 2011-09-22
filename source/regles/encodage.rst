.. _encodage:

#######################
L'encodage des fichiers
#######################

L'encodage est un jeu de caractère qui permet de "transcrire des langues naturelles
avec une représentation numérique pour chaque caractère" (wikipédia)

Pour la france, les jeux de caractères sont les suivants (source wikipédia) ::

    * ASCII, standard de compatibilité, sans accent.
    * Windows-1252, pour Windows, encore appelé CP1252 ou Ansinew
    * ISO 8859-1, avant l'euro (latin-1 ou europe occidentale)
    * ISO 8859-15, après l'euro (latin-9 ou latin-1)
    * Unicode, voir aussi UTF-8 (reprend tous les caractères des jeux de code précédent et se présente comme le standard).

La version 4.1.0 corrige les bugs en utf8 de la version 4.0.0.





encodage écran
==============

le test a été effectué avec 2 normes :

    ISO-8859-1
    UTF8

Attention, fpdf ne supporte pas utf8 et des modifications ont été faites dans
la version 4.01 du framewrk pour qu'utf8 soit implémenté (commande utf8_decode et encode)

De la même manière, des bugs ont été corrigés dans la recherche table et dans
les sous formulaire (évolution openMairie 4.1.0)


voir framework/paramètrage/locales.inc


encodage postgresql
===================


information geographique/install postgis

Il est possible de conserver l'option sql_ascii en création de base avec la version
postgresql 8.4 en rajoutant l'option T ::

    createdb -E SQL_ASCII -T template0 openelec
    
    
traduction
==========

Il vaut mieux mettre les accents dans les traductions plutôt que
de les intégrer dans les zones à traduire et éviter ainsi les problèmes
d'encodage.

*voir outils/poedit*

