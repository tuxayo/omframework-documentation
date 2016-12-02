.. _edition:

################
Module 'Édition'
################

.. contents::

============
Introduction
============

Le framework openMairie permet d'effectuer des éditions au format PDF. Ce module est composé de trois éléments fonctionnels :

* les états et lettres types,
* les listings,
* les étiquettes.



==========================
Les états et lettres types
==========================

C'est la fonctionnalité la plus évoluée du module 'Édition', elle permet à l'utilisateur final de composer directement depuis l'interface du logiciel des éditions complexes au format PDF comme des factures, des courriers, des procès verbaux, ... Il est possible d'insérer dans ces éditions : un logo, des sous-états, des champs de fusion.



Paramétrer des états
--------------------

Il est conseillé d'utiliser l'assistant état du générateur.

Les paramètres sont les suivants ::

    orientation portrait ou paysage
    format="A4", A3
    position et nom  du logo 
    titre de l état
    position et caractéristiques du titre
    corps de l état
    position et caractéristiques du corps
    la requête SQL
    les sous-états associés et les caractéristiques




Pour le corps et le titre, les zones entre crochets (exemple [nom]) sont les
champs de fusion, sélectionnés par la requête SQL. 

Ces champs de fusion peuvent être mis en majucule ou en minuscule selon les 
besoins grâce aux balises <MAJ></MAJ> et  <min></min>.

EX. :

Requête SQL = SELECT nom as nom FROM A;

Cette phrase dans un PDF 
'Lorem [nom] dolor sit amet, <MAJ>[nom]</MAJ> adipiscing elit. Curabitur feugiat.'
deviendra
'Lorem nom dolor sit amet, NOM adipiscing elit. Curabitur feugiat.'

Les variables commençant par "&" sont celles définies dans dyn/varpdf.inc
(exemple &aujourdhui) et dans la table om_parametre.

- ``obj/om_etat.class.php``
- ``core/obj/om_etat.class.php``
- ``gen/obj/om_etat.class.php``
- ``sql/pgsql/om_etat.form.inc.php``
- ``sql/pgsql/om_etat.inc.php``
- ``core/sql/pgsql/om_etat.form.inc.php``
- ``core/sql/pgsql/om_etat.inc.php``
- ``gen/sql/pgsql/om_etat.form.inc.php``
- ``gen/sql/pgsql/om_etat.inc.php``




Paramétrer des lettres-type
---------------------------

Il est conseillé d'utiliser l'assistant lettre-type du générateur.

Les paramètres sont les suivants ::

    orientation portrait ou paysage
    format="A4", A3
    position et nom  du logo 
    titre de la lettre
    position et caractéristiques du titre
    corps de la lettre
    position et caractéristiques du corps
    la requête SQL


Pour le corps et le titre, les zones entre crochets  sont les champs de fusion,
sélectionnés par la requête.

Les variables commençant par "&" sont celles définies dans
dyn/varlettretypepdf.inc (exemple &aujourdhui) et dans la table om_parametre.


- ``obj/om_lettretype.class.php``
- ``core/obj/om_lettretype.class.php``
- ``gen/obj/om_lettretype.class.php``
- ``sql/pgsql/om_lettretype.form.inc.php``
- ``sql/pgsql/om_lettretype.inc.php``
- ``core/sql/pgsql/om_lettretype.form.inc.php``
- ``core/sql/pgsql/om_lettretype.inc.php``
- ``gen/sql/pgsql/om_lettretype.form.inc.php``
- ``gen/sql/pgsql/om_lettretype.inc.php``



Actif, non actif
----------------

Les sous-etats sont liés a un ou plusieurs état.

Les états, sous-états, et lettre type peuvent être "actif" ou "non-actif".

Par défaut sont pris en compte :

1 - l'édition  "actif" de la collectivité

2 - l'édition "actif" de la multicollectivité

3 - l'édition "non-actif" de la multicollectivité


Les éditions d'une collectivité ayant le statut "non-actif" ne sont pas prises
en compte.


Les requêtes
------------

Description
===========

Une requête peut être :

- de type SQL
- de type OBJET

---
SQL
---

Lorsqu'elle est de type SQL il s'agit d'un ordre *SELECT* dont les colonnes récupérées seront les champs de fusion.
La clause *WHERE* permet de filtrer sur l'enregistrement adéquat.
Prenons par exemple cette requête :

.. code-block:: sql

  SELECT employe.nom as nom_employe FROM employe WHERE employe.id = &idx

Elle est liée à la lettre-type *fiche_employe* comportant le champ de fusion *[nom_employe]*.
Pour générer l'édition depuis l'objet métier *employe* on appelle :


.. code-block:: php

  $pdf = $this->compute_pdf_output('lettretype', 'fiche_employe', null, $this->getVal($this->clePrimaire));


.. note::

  Mettre à jour les colonnes de la requête SQL (*om_requete.requete*)
  oblige à corriger les champs de fusion (*om_requete.merge_fields*)
  afin de proposer une aide à la saisie cohérente lors de la rédaction d'une lettre-type.


-----
OBJET
-----

Dans le cas d'une requête objet les champs *om_requete.requete* et *om_requete.merge_fields* ne sont plus utilisés.
Ce sont les méthodes de la classe *dbForm* *get_values_merge_fields()* et *get_labels_merge_fields()*
qui retournent respectivement les champs de fusion et l'aide à la saisie.
Pour reprendre l'exemple de la requête SQL, il faut dans ce cas spécifier la classe *employe* (*om_requete.classe*).
Nous aurons automatiquement accès à tous ses champs.
Les booléens sont transformés en oui/non, les dates sont formatées en jj/mm/aaaa et les libellés des clés étrangères sont récupérés.

.. note::

  Si un employé est rattaché à une entreprise elle-même liée à une ville,
  alors définir la classe *employe;entreprise;ville* permettra de récupérer
  les champs de tous ces objets.

On peut également disposer de notre propre méthode (*om_requete.methode*) pour construire manuellement les champs de fusion.
Celle-ci doit avoir un argument de type *string*. Il peut prendre la valeur de *values* ou *labels* selon où le framework y fait appel.
Ainsi pour une requête objet dont on ne veut que le nom de l'employe il faut créer la méthode suivante :

.. code-block:: php

  public function get_only_name($type) {
    switch ($type) {
        case 'values':
            //
            $values = array();
            $values['employe.nom'] = $this->getVal('nom');
            return $values;
        case 'labels':
            //
            $labels = array();
            $labels['employe']['employe.nom'] = _("nom de l'employé");
            return $labels;
    }
  }

Le tableau des labels a une dimension supplémentaire.
Cela permet de catégoriser les champs de fusions proposés dans l'aide à la saisie (un tableau HTML par catégorie).


Modèle de données
=================

.. code-block:: sql

   CREATE TABLE om_requete
   (
     om_requete integer NOT NULL, -- Identifiant unique
     code character varying(50) NOT NULL, -- Code de la requête
     libelle character varying(100) NOT NULL, -- Libellé de la requête
     description character varying(200), -- Description de la requête
     requete text, -- Requête SQL
     merge_fields text, -- Champs de fusion
     type character varying(200) NOT NULL, -- Requête SQL ou objet ?
     classe character varying(200), -- Nom de(s) la classe(s) contenant la méthode
     methode character varying(200), -- Méthode (de la première classe si plusieurs définies) fournissant les champs de fusion. Si non spécifiée appel à une méthode générique
     CONSTRAINT om_requete_pkey PRIMARY KEY (om_requete)
   );


- ``obj/om_requete.class.php``
- ``sql/pgsql/om_requete.form.inc.php``
- ``sql/pgsql/om_requete.inc.php``
- ``core/obj/om_requete.class.php``
- ``core/sql/pgsql/om_requete.form.inc.php``
- ``core/sql/pgsql/om_requete.inc.php``
- ``gen/obj/om_requete.class.php``
- ``gen/sql/pgsql/om_requete.form.inc.php``
- ``gen/sql/pgsql/om_requete.inc.php``


Les sous-états
--------------

Il est conseillé d'utiliser l'assistant sous-etat du générateur.

Les paramètres  sont les suivants ::

    texte et caractéristique du Titre
    Intervalle avant et après le tableau
    Entête de tableau (nom de colonne)
    caractéristique du tableau
    caractéristique des cellules
    total, moyenne, nombre
    requête SQL


Pour le titre, les zones entre crochets sont les champs de fusion,
sélectionnés par la requête.

Les variables commençant par "&" sont celles définies dans dyn/varpdf.inc
(exemple &aujourdhui) et dans la table om_parametre.

- ``obj/om_sousetat.class.php``
- ``core/obj/om_sousetat.class.php``
- ``gen/obj/om_sousetat.class.php``
- ``sql/pgsql/om_sousetat.form.inc.php``
- ``sql/pgsql/om_sousetat.inc.php``
- ``core/sql/pgsql/om_sousetat.form.inc.php``
- ``core/sql/pgsql/om_sousetat.inc.php``
- ``gen/sql/pgsql/om_sousetat.form.inc.php``
- ``gen/sql/pgsql/om_sousetat.inc.php``



Les champs de fusion
--------------------




Les variables de remplacement
-----------------------------

Lorsque dans les zones de remplacement des éditions, une chaîne de caractère commençant par "&" est identifiée elle essaye d'être remplacée. Ces éléments sont nommées variables de remplacement. Elles peuvent provenir de trois sources différentes : 

- les fichiers de configuration ``../dyn/var*pdf.inc``,
- les méthodes globales de la classe du fichier ``../obj/om_dbform.class.php``,
- la table de paramètres ``om_parametre``.


Les fichiers de configuration ``../dyn/var*pdf.inc``
====================================================


Les méthodes globales de la classe du fichier ``../obj/om_dbform.class.php``
============================================================================


La table de paramètres ``om_parametre``
=======================================

``prefixe_edition_substitution_vars`` doit être définit.



Les logos
---------

- ``obj/om_logo.class.php``
- ``core/obj/om_logo.class.php``
- ``gen/obj/om_logo.class.php``
- ``sql/pgsql/om_logo.form.inc.php``
- ``sql/pgsql/om_logo.inc.php``
- ``core/sql/pgsql/om_logo.form.inc.php``
- ``core/sql/pgsql/om_logo.inc.php``
- ``gen/sql/pgsql/om_logo.form.inc.php``
- ``gen/sql/pgsql/om_logo.inc.php``




L'éditeur WYSIWYG
-----------------

Description de l'intégration de TinyMCE et des différents configurations.


Les anciens fichiers de paramétrage
-----------------------------------

Les fichiers de paramétrage ``../sql/pgsql/<OBJ>.etat.inc.php``, ``../sql/pgsql/<OBJ>.etat.inc``, ``../sql/pgsql/<OBJ>.lettretype.inc.php`` ou ``../sql/pgsql/<OBJ>.lettretype.inc`` sont les anciens fichiers de paramétrage des éditions. Ils ne peuvent plus être utilisé depuis la version 4.0 du framework.

Un système d'import est disponible dans le générateur pour transformer ces anciens fichiers de paramétrage en enregistremment selon le nouveau format de paramétrage.

La prévisualisation
-------------------

Le bouton Prévisualiser permet, pour une lettre type ou un état, d'avoir un apercu du document qui sera généré. Les champs de fusion ne seront pas interprétés.


============
Les listings
============

Description de la fonctionnalité
--------------------------------




Le fichier de paramétrage ``../sql/pgsql/<OBJ>.pdf.inc.php``
------------------------------------------------------------

Un état PDF peut être généré par le générateur (option).

Les paramètres sont les suivants ::

    texte et caractéristique du Titre
    Entête de tableau (nom de colonne)
    caractéristique du tableau
    caractéristique des cellules
    total, moyenne, nombre
    requête SQL



==============
Les étiquettes
==============

Description de la fonctionnalité
--------------------------------



Le fichier de paramétrage ``../sql/pgsql/<OBJ>.pdfetiquette.inc.php``
---------------------------------------------------------------------





==========
Composants
==========

Les scripts du framework qui s'occupent de la gestion des éditions sont :

- ``core/fpdf_etat.php``
- ``core/fpdf_etiquette.php``
- ``core/db_fpdf.php``
- ``core/om_edition.class.php``
- ``scr/edition.php``


Les librairies PHP sont :

- ``php/fpdf/``
- ``php/tcpdf/``

