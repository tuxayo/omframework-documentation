.. _om_sig_map_comp:


======================
Saisie des géométries:
======================

om_sig_map_comp permet d'associer un ou plusieurs champs géométriques lié à la table
correspondante au formulaire concernés.

Ces champs géométriques peuvent être mis à jour dans l'interface.

Ces champs constituent la couche vectorielle (modifiable) de la carte.

Il est possible de lister les géométries concernés dans le menu  administration ->
option om_sig_map onglet, om_sig_map_comp.

Le champ géométrique à mettre à jour se fait dans un sous formulaire d'om_sig_map 

.. image:: tab_om_sig_map_comp.png

Formulaire
==========


Il est possible de modifier / supprimer les géeométries dans le sous formulaire de saisie om_sig_map_comp
en appuyant sur modifier ou supprimer

.. image:: form_om_sig_map_comp.png

les champs suivants peuvent etre mis a jour :

.. note::

	Le champ *'om_sig_map_comp'* est un champ numerique entier non obligatoire.

	Le champ *'om_sig_map'* est un champ numerique entier non obligatoire.

	Le champ *'libelle'* est un champ libelle non obligatoire de 50 caractere(s) .

	Le champ *'obj_class'* est un champ libelle non obligatoire de 100 caractere(s) .

	Le champ *'ordre'* est un champ numerique entier non obligatoire.

	Le champ *'actif'* est un champ booleen obligatoire.

	Le champ *'comp_maj'* est un champ booleen obligatoire.

	Le champ *'comp_table_update'* est un champ libelle obligatoire de 30 caractere(s) .

	Le champ *'comp_champ_idx'* est un champ libelle obligatoire de 30 caractere(s) .

	Le champ *'comp_champ'* est un champ libelle obligatoire de 30 caractere(s) .

	Le champ *'type_geometrie'* est un champ libelle obligatoire de 30 caractere(s) .

	Il y a une contrainte  de cle primaire  dont le nom est *'om_sig_map_comp_pkey'*.

	Il y a une contrainte  de cle scondaire  dont le nom est *'om_sig_map_comp_om_sig_map_fkey'*.



Description des champs :
========================

Le champ om_sig_map_comp est la clé primaire numérique automatique

om_sig_map est la clé primaire de la carte concernée

Il faut saisir le nom de la géométrie (champ géométrique) et l'objet concerné

Si la mise à jour est autorisée (vrai), il faut saisir

- la table du champ géométrique à modifier

- le nom de champ correspondant à la clé primaire de l'enregistrement à modifier

- le champ géographique à modifier

- le type de géométrie




