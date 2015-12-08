.. _administration:

##############
Administration
##############

Cette rubrique est dédiée à l'administration des fonctionnalités disponibles
depuis l'interface : les tableaux de bord, les éditions, le module om_sig, 
...

====================
Les tableaux de bord
====================

Widget
------

...


Composition
-----------

...

============
Les éditions
============

États et lettres types
----------------------


.. warning::

   Cette rubrique est en cours de rédaction.


.. _editions_etat_lettretype_positionnement:

Positionnement des éléments dans l'édition
==========================================

.. image:: editions_etat_lettretype_positionnement.png

1. Marge gauche (en millimètre) de l'édition depuis la limite gauche de la page (voir :ref:`editions_etat_lettretype_bloc_edition`) qui concerne les blocs : :ref:`editions_etat_lettretype_bloc_en-tete`, :ref:`editions_etat_lettretype_bloc_corps`, :ref:`editions_etat_lettretype_bloc_pied-de-page`.
2. Marge haute (en millimètre) de l'édition depuis la limite haute de la page (voir :ref:`editions_etat_lettretype_bloc_edition`) qui concerne le bloc : :ref:`editions_etat_lettretype_bloc_corps`.
3. Marge droite (en millimètre) de l'édition depuis la limite droite de la page (voir :ref:`editions_etat_lettretype_bloc_edition`) qui concerne les blocs : :ref:`editions_etat_lettretype_bloc_en-tete`, :ref:`editions_etat_lettretype_bloc_corps`, :ref:`editions_etat_lettretype_bloc_pied-de-page`.
4. Marge basse (en millimètre) de l'édition  depuis la limite basse de la page (voir :ref:`editions_etat_lettretype_bloc_edition`) qui concerne le bloc : :ref:`editions_etat_lettretype_bloc_corps`.
5. Espacement (en millimètre) entre le plafond du bloc 'en-tête' et la limite haute de la page (voir :ref:`editions_etat_lettretype_bloc_en-tete`).
6. Espacement (en millimètre) entre le plafond du bloc 'pied de page' et la limite basse de la page (voir :ref:`editions_etat_lettretype_bloc_pied-de-page`).
7. position du coin haut/gauche du logo par rapport au coin haut/gauche de l'édition (voir :ref:`editions_etat_lettretype_bloc_edition`).
8. position du coin haut/gauche du logo par rapport au coin haut/gauche de l'édition (voir :ref:`editions_etat_lettretype_bloc_edition`).
9. Espacement (en millimètre) entre la paroi gauche du bloc 'titre' et la limite gauche de la page (voir :ref:`editions_etat_lettretype_bloc_titre`).
10. Espacement (en millimètre) entre le plafond du bloc 'titre' et la limite haute de la page  (voir :ref:`editions_etat_lettretype_bloc_titre`).
11. Largeur (en millimètre) du bloc 'titre' depuis la paroi gauche du bloc 'titre' (voir :ref:`editions_etat_lettretype_bloc_titre`).


.. _editions_etat_lettretype_bloc_edition:

Bloc 'édition'
==============

.. image:: editions_etat_lettretype_bloc_edition.png

Les informations d'**édition** à saisir sont :

* **id** : identifiant de l'état/lettre type.
* **libellé** : libellé affiché dans l'application lors de la sélection d'une édition.
* **actif** : permet de définir si l'édition est active ou non.

.. note::

    Les champs **id** et **libellé** sont obligatoires, les **id** actif sont uniques.


--------------------------------
Paramètres généraux de l'édition
--------------------------------

Les champs de **paramètres généraux de l'édition** à saisir sont :

* **Orientation** : orientation de l'édition (portrait/paysage).
* **Format** : format de l'édition (A4/A3).
* **Logo** : sélection du logo depuis la table des logos configurés.
* **Logo haut** : position du coin haut/gauche du logo par rapport au coin haut/gauche de l'édition (voir 8 dans :ref:`editions_etat_lettretype_positionnement`).
* **Logo gauche** : position du coin haut/gauche du logo par rapport au coin haut/gauche de l'édition (voir 7 dans :ref:`editions_etat_lettretype_positionnement`).
* **Marge gauche** : Marge gauche (en millimètre) de l'édition depuis la limite gauche de la page (voir 1 dans :ref:`editions_etat_lettretype_positionnement`).
* **Marge haut** : Marge haute (en millimètre) de l'édition depuis la limite haute de la page (voir 2 dans :ref:`editions_etat_lettretype_positionnement`).
* **Marge droite** : Marge droite (en millimètre) de l'édition depuis la limite droite de la page (voir 3 dans :ref:`editions_etat_lettretype_positionnement`).
* **Marge bas** : Marge basse (en millimètre) de l'édition  depuis la limite basse de la page (voir 4 dans :ref:`editions_etat_lettretype_positionnement`).


.. _editions_etat_lettretype_bloc_en-tete:

Bloc 'en-tête'
==============

.. image:: editions_etat_lettretype_bloc_en-tete.png

Ce bloc est facultatif et a pour particularité de se répéter sur chaque page.

* **Espacement** : Espacement (en millimètre) entre le plafond du bloc 'en-tête' et la limite haute de la page (voir 5 dans :ref:`editions_etat_lettretype_positionnement`).
* **En-tête** : éditeur riche permettant une mise en page complexe.

Limitation(s) :

- les marges gauche et droite ne sont pas dissociables de celles du corps,
- la hauteur n'est pas fixable, elle dépend du contenu positionné à l'intérieur.


.. _editions_etat_lettretype_bloc_titre:

Bloc 'titre'
============

.. image:: editions_etat_lettretype_bloc_titre.png

Ce bloc est obligatoire.

* **Titre** : éditeur riche permettant une mise en page complexe.

--------------------------------
Paramètres du titre de l'édition
--------------------------------

Positionnement :

* **Titre gauche** : Espacement (en millimètre) entre la paroi gauche du bloc 'titre' et la limite gauche de la page (voir 9 dans :ref:`editions_etat_lettretype_positionnement`).
* **Titre haut** : Espacement (en millimètre) entre le plafond du bloc 'titre' et la limite haute de la page (voir 10 dans :ref:`editions_etat_lettretype_positionnement`).
* **Largeur de titre** : Largeur (en millimètre) du bloc 'titre' depuis la paroi gauche du bloc 'titre' (voir 11 dans :ref:`editions_etat_lettretype_positionnement`).
* **Hauteur** : hauteur minimum du titre.

Bordure :

* **bordure** : Affichage ou non d'une bordure.


.. _editions_etat_lettretype_bloc_corps:

Bloc 'corps'
============

.. image:: editions_etat_lettretype_bloc_corps.png

Ce bloc est obligatoire.

* **Corps** : éditeur riche permettant une mise en page complexe.

-------------------------
Paramètres des sous-états
-------------------------

* **Police personnalisée** : sélection de la police des sous-états.
* **Couleur texte** : sélection de la couleur du texte des sous-états.


.. _editions_etat_lettretype_bloc_pied-de-page:

Bloc 'pied de page'
===================

.. image:: editions_etat_lettretype_bloc_pied-de-page.png

Ce bloc est facultatif et a pour particularité de se répéter sur chaque page.

* **Espacement** : Espacement (en millimètre) entre le plafond du bloc 'pied de page' et la limite basse de la page (voir 6 dans :ref:`editions_etat_lettretype_positionnement`).
* **Pied de page** : éditeur riche permettant une mise en page complexe.

Limitation(s) :

- les marges gauche et droite ne sont pas dissociables de celles du corps,
- la hauteur n'est pas fixable, elle dépend du contenu positionné à l'intérieur.


.. _editions_etat_lettretype_bloc_champs-de-fusion:

Bloc 'champs de fusion'
=======================

.. image:: editions_etat_lettretype_bloc_champs-de-fusion.png

* **Requête** : sélection d'un jeu de champs de fusion.
* **Champs de fusion** : Liste des champs de fusion disponibles pour la requête sélectionnée.
* **Variables de remplacement** : Liste des variables de remplacements disponibles.


.. _editions_etat_lettretype_editeur-texte-riche:

Aide à la saisie dans les éditeurs de texte riche
=================================================

.. _editions_etat_lettretype_editeur-texte-riche-configurations:

--------------------------
Configurations disponibles
--------------------------

Trois configurations différentes de l'éditeur de texte riche sont utilisées :

- configuration n°1 : :ref:`editions_etat_lettretype_bloc_corps`,
- configuration n°2 : :ref:`editions_etat_lettretype_bloc_en-tete`, :ref:`editions_etat_lettretype_bloc_titre`, :ref:`editions_etat_lettretype_bloc_pied-de-page`,
- configuration n°3 : blocs de texte avec une mise en forme limitée toujours destinés à être intégré dans une édition via un champs de fusion.


+--------------------------------------------------------------------------------+--------+--------+--------+
| Fonction / Configuration                                                       | N°1    | N°2    | N°3    |
+================================================================================+========+========+========+
| :ref:`editions_etat_lettretype_editeur-texte-riche-tableaux`                   | x      | x      |        |
+--------------------------------------------------------------------------------+--------+--------+--------+
| :ref:`editions_etat_lettretype_editeur-texte-riche-tableaux-insecables`        | x      |        |        |
+--------------------------------------------------------------------------------+--------+--------+--------+
| :ref:`editions_etat_lettretype_editeur-texte-riche-sauts-de-page`              | x      |        |        |
+--------------------------------------------------------------------------------+--------+--------+--------+
| :ref:`editions_etat_lettretype_editeur-texte-riche-code-barres`                | x      | x      |        |
+--------------------------------------------------------------------------------+--------+--------+--------+
| :ref:`editions_etat_lettretype_editeur-texte-riche-majuscule-minuscule`        | x      | x      | x      |
+--------------------------------------------------------------------------------+--------+--------+--------+
| :ref:`editions_etat_lettretype_editeur-texte-riche-sous-etats`                 | x      |        |        |
+--------------------------------------------------------------------------------+--------+--------+--------+
| :ref:`editions_etat_lettretype_editeur-texte-riche-plein-ecran`                | x      | x      |        |
+--------------------------------------------------------------------------------+--------+--------+--------+
| :ref:`editions_etat_lettretype_editeur-texte-riche-code-source`                | x      | x      | x      |
+--------------------------------------------------------------------------------+--------+--------+--------+
| :ref:`editions_etat_lettretype_editeur-texte-riche-correction-orthographique`  | x      | x      | x      |
+--------------------------------------------------------------------------------+--------+--------+--------+


.. _editions_etat_lettretype_editeur-texte-riche-tableaux:

--------------------
Gestion des tableaux
--------------------

Cette aide à la saisie n'est pas nécessairement disponible dans toutes les configurations de l'éditeur de texte riche (voir :ref:`editions_etat_lettretype_editeur-texte-riche-configurations`).

* **Créer un tableau** :

Choisir le nombre de lignes et de colonnes du tableau.

.. note::

    Il faut bien placer le curseur dans une des cellules du tableau que l'on 
    souhaite paramétrer.
    Idem pour le paramétrage des lignes et colonnes.

* **Paramétrage général du tableau** :

    - Largeur :
     
    Ce champ sert à indiquer la largeur du tableau en % (UNIQUEMENT) par rapport 
    à la largeur du PDF.
         
    Par exemple, si le PDF fait une largeur de 30 cm et que la lageur du tableau    
    est de 10%, le tableau fera 3 cm de largeur sur le PDF.
     
    - Hauteur :
         
    Ce champ sert à indiquer la hauteur du tableau en % (UNIQUEMENT) par rapport 
    à la hauteur du PDF.
         
    Par exemple, si le PDF fait une hauteur de 50 cm et que la hauteur du tableau    
    est de 25%, le tableau fera 12.5 cm de hauteur sur le PDF.
     
    - Espacement inter-cellules :
    
    Espacement entre les cellules. En pixel.
    
    - Espace interne cellule :
    
    Espacement entre les bords de la cellule et son contenu. En pixel.
    
    - Bordure :
    
    Epaisseur des bordures du tableau. En pixel.
    
    - Titre :
    
    Lorsque cette case est cochée, elle permet de rajouter un titre au tableau.
    
    - Alignement :
    
    Permet de choisir le type d'alignement du texte dans le tableau. 
    Valeurs possibles : n/a (aucun), Gauche, Centré, Droite.

* **Supprimer un tableau**


* **Paramétrage des cellules** :

    - Largeur :
    
    Ce champ sert à indiquer la largeur de la colonne en % (UNIQUEMENT) par 
    rapport à la largeur du tableau.
         
    Par exemple, si le tableau fait une largeur de 30 cm et que la largeur de la 
    colonne est de 10%, la colonne fera 3 cm de largeur.
    
    - Hauteur :
    
    Ce champ sert à indiquer la hauteur de la colonne en % (UNIQUEMENT) par 
    rapport à la hauteur du tableau.
         
    Par exemple, si le tableau fait une hauteur de 50 cm et que la hauteur de la
    colonne est de 25%, la colonne fera 12.5 cm de hauteur.
    
    - Type de cellule :
    
    Permet de définir si c'est une cellule "normale" ou une cellule qui va servir 
    d'en-tête dans le tableau.
    Valeurs possibles : Cellule, Cellule d'en-tête.
    
    - Étendue :
    
    Paramètre sur quoi doivent s'appliquer les paramètres renseignés.
    Valeurs possibles : n/a (aucun), Ligne, Colonne, Groupe de lignes, Groupe de 
    colonnes.
    
    - Alignement :
    
    Permet de choisir le type d'alignement du texte dans la cellule. 
    Valeurs possibles : n/a (aucun), Gauche, Centré, Droite.

* **Fusionner des cellules** :

En sélectionnant les cellules à fusionner et en cliquant sur 
Tableau → Cellule → Fusionner les cellules les cellules seront fusionnées.

Si aucune cellule n'est sélectionnée, un menu apparaît :

    - Colonnes :
    
    Nombre de colonnes qui vont être fusionnées à partir de la cellule dans 
    laquelle le curseur est positionné.
    
    - Lignes :
    
    Nombre de lignes qui vont être fusionnées à partir de la cellule dans 
    laquelle le curseur est positionné.


* **Diviser les cellules** :

Divise la cellule dans laquelle le curseur est positionné si elle avait été 
fusionnée avant.


* **Paramétrage des lignes** :

    - Type de ligne :
    
    Permlet de définir le type de la ligne.
    Valeurs possibles : En-tête, Corps, Pied.
    
    - Alignement :

    Permet de choisir le type d'alignement du texte dans la ligne. 
    Valeurs possibles : n/a (aucun), Gauche, Centré, Droite.

    - Hauteur : 

    Ce champ sert à indiquer la hauteur de la ligne en % (UNIQUEMENT) par 
    rapport à la hauteur du tableau.
         
    Par exemple, si le tableau fait une hauteur de 50 cm et que la hauteur de la
    ligne est de 25%, la ligne fera 12.5 cm de hauteur.


* **Insérer une ligne** :

Permet d'insérer une ligne avant ou après la ligne sur laquelle le curseur est 
positionné.


* **Effacer une ligne** :

Supprimer la ligne sur laquelle le curseur est positionné.

* **Couper une ligne** :

Coupe la ligne sur laquelle le curseur est positionné.

* **Copier une ligne** :

Copie la ligne sur laquelle le curseur est positionné.


* **Coller une ligne** :

Colle la ligne qui avait été copiée/coupée avant ou après la ligne sur laquelle 
le curseur est positionné.


* **Insérer une colonne** :

Insère une colonne avant ou après la colonne sur laquelle le curseur est 
positionné.


* **Effacer une colonne** :

Supprime la colonne sur laquelle le curseur est positionné.


.. _editions_etat_lettretype_editeur-texte-riche-tableaux-insecables:

-------------------
Tableaux insécables
-------------------

Cette aide à la saisie n'est pas nécessairement disponible dans toutes les configurations de l'éditeur de texte riche (voir :ref:`editions_etat_lettretype_editeur-texte-riche-configurations`).

...


.. _editions_etat_lettretype_editeur-texte-riche-sauts-de-page:

-------------------------
Gestion des sauts de page
-------------------------

Le saut de page permet d'insérer un marqueur dans l'édition PDF, pour que le contenu qui suit soit positionné sur la page suivante.

Cette aide à la saisie n'est pas nécessairement disponible dans toutes les configurations de l'éditeur de texte riche (voir :ref:`editions_etat_lettretype_editeur-texte-riche-configurations`).

Pour ajouter un saut de page, il faut :

* positionner le curseur là où l'on souhaite l'insérer,
* cliquer sur l'élément 'Saut de page' du menu déroulant de l'éditeur de texte riche 'Insérer'.

Dans l'éditeur apparaît un rectangle avec des bordures en pointillés.



.. _editions_etat_lettretype_editeur-texte-riche-code-barres:

-----------------------
Gestion des code-barres
-----------------------

Cette aide à la saisie n'est pas nécessairement disponible dans toutes les configurations de l'éditeur de texte riche (voir :ref:`editions_etat_lettretype_editeur-texte-riche-configurations`).

Saisir le champ de fusion

Sélectionner le champ de fusion

Cliquer sur le bouton de génération du code-barres puis valider le formulaire 
pour enregistrer les changements


.. _editions_etat_lettretype_editeur-texte-riche-majuscule-minuscule:

-------------------
Majuscule/Minuscule
-------------------

Cette aide à la saisie n'est pas nécessairement disponible dans toutes les configurations de l'éditeur de texte riche (voir :ref:`editions_etat_lettretype_editeur-texte-riche-configurations`).

...

.. _editions_etat_lettretype_editeur-texte-riche-plein-ecran:

---------------------------
Gestion du mode plein écran
---------------------------

Cette aide à la saisie n'est pas nécessairement disponible dans toutes les configurations de l'éditeur de texte riche (voir :ref:`editions_etat_lettretype_editeur-texte-riche-configurations`).

...

.. _editions_etat_lettretype_editeur-texte-riche-code-source:

-----------
Code source
-----------

Cette aide à la saisie n'est pas nécessairement disponible dans toutes les configurations de l'éditeur de texte riche (voir :ref:`editions_etat_lettretype_editeur-texte-riche-configurations`).

...

.. _editions_etat_lettretype_editeur-texte-riche-correction-orthographique:

-------------------------
Correction orthographique
-------------------------

Cette aide à la saisie n'est pas nécessairement disponible dans toutes les configurations de l'éditeur de texte riche (voir :ref:`editions_etat_lettretype_editeur-texte-riche-configurations`).

...

.. _editions_etat_lettretype_editeur-texte-riche-sous-etats:

-----------------------
Insertion de sous-états
-----------------------

Cette aide à la saisie n'est pas nécessairement disponible dans toutes les configurations de l'éditeur de texte riche (voir :ref:`editions_etat_lettretype_editeur-texte-riche-configurations`).

...


Sous-états
----------


Requêtes
--------


Logos
-----


======================
Usage du module om_sig
======================

Ce document a pour objet de décrire le module sig interne d'openMairie dans la version om 4.4.5.



Dans sa version 4.4.5 ;

- intégration des formulaires dans le sig interne

- integration des résultats du moteur de recherche dans les cartes (cas utilisation moteur de recherche) 

- intégration dans les cartes d'un résultat dans reqmo (cas d'utilisation reqmo)

- accès multiples aux objets

- accès à des objets multi géométrie


La nouveauté de la version 4.4.5 du sig interne est la mise en place d'une nouvelle ergonomie avec
un cartouche où sont accessibles toutes les commandes.



.. toctree::

    ergonomie.rst
    couche.rst
    info.rst
    boite_outil.rst
    edition.rst

============================
Paramétrage du module om_sig
============================

Il est décrit ici le paramétrage du sig interne.

Il est décrit les principes ainsi que les différents formulaires du sig interne.

Ces formulaires sont accessibles dans le menu option adminitration.

.. toctree::

    principe
    om_sig_map.rst
    om_sig_flux.rst
    om_sig_map_comp.rst
    om_sig_map_flux.rst
    om_sig_extent.rst

