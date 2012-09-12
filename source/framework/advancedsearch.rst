====================
La recherche avancée
====================

Les différents types de recherche
=================================

Recherche simple
----------------

Cette recherche est celle disponible par défaut sur les tableaux d'openMairie.

Elle permet de:

- rechercher une valeur dans une colonne parmi toutes celles affichées;
- rechercher une valeur dans toutes les colonnes affichées;
- rechercher des valeurs approximatives.

.. note::
   Il est possible de modifier la liste des colonnes dans laquelle est effectuée
   la recherche. Cette liste n'est donc pas forcément celle affichéee. Elle
   l'est seulement par défaut, c'est à dire lorsqu'aucune surcharge ne modifie
   les fichiers générés dans ``gen/sql/``.

Recherche avancée
-----------------

Cette recherche est une fonctionnalité qui peut être activée et configurée
manuellement pour un ou plusieurs tableaux donnés.

Elle permet de:

- afficher un formulaire de recherche mono-critère permettant d'effectuer des
  recherches strictes ou approximatives;
- afficher un formulaire de recherche multi-critères permettant d'effectuer
  des recherches strictes ou approximatives;
- rechercher des valeurs dans des tables et des colonnes qui ne sont pas
  affichées.

Recherche avancée mono-critère
..............................

Le formulaire de recherche avancée mono-critère est un formulaire ne s'affichant
que si la recherche avancée est activée. Il permet aux utilisateurs de basculer
sur un formulaire similaire à celui de recherche simple lorsque la recherche
avancée est activée.

Ce formulaire se comporte de la même manière que celui de recherche simple, avec
quelques différences:

- il permet de rechercher des valeurs strictes ou approximatives;
- il conserve les valeurs recherchées après la réalisation d'une action (ajout,
  modification, etc...);
- il dispose d'un bouton ``Vider le formulaire`` permettant de vider les champs;
- il dispose d'un bouton ``+`` permettant de basculer sur le formulaire
  multi-critères.

Recherche avancée multi-critères
................................

Le formulaire de recherche avancée multi-critères est un formulaire ne
s'affichant que si la recherche avancée est activée. Il permet aux utilisateurs
de bénéficier de plusieurs champs, et ainsi effectuer des recherches plus
précise qu'avec le formulaire de recherche simple.

Description du formulaire:

- il peut afficher plusieurs champs, de type texte, nombre, date ou liste à
  choix;
- il permet, pour chaque tableau, de configurer la liste des champs affichés;
- il permet, pour chaque champ, de rechercher des valeurs strictes ou
  approximatives;
- il permet, pour chaque champ, de rechercher des valeurs dans des tables et
  des colonnes qui ne sont pas affichées;
- il conserve les valeurs recherchées après la réalisation d'une action (ajout,
  modification, etc...);
- il dispose d'un bouton ``Vider le formulaire`` permettant de vider les champs;
- il dispose d'un bouton ``+`` permettant de basculer sur le formulaire
  mono-critère.

Activer et configurer la recherche avancée
==========================================

Exemple avec le modèle ``om_utilisateur``.

Pour activer la recherche avancée, rendez-vous dans le fichier
``sql/sgbd/om_utilisateur.inc.php`` et ajoutez la configuration suivante au
tableau d'options:

.. code-block:: php

   <?php

    $options[] =  array('type' => 'search',
                        'display' => true,
                        'advanced' => $champs,
                        'default_form' => 'advanced',
                        'absolute_object' => 'om_utilisateur');

   ?>

.. note::
   A partir de la version 4.3.0 d'openMairie, le tableau ``$options`` est
   disponible dans les fichiers ``sql/`` de l'application. Il n'est plus
   nécessaire de le déclarer manuellement.

La clé ``type`` est obligatoire. Elle permet de definir le type de l'option.
Pour une recherche il faut saisir ``search``.

La clé ``display`` est obligatoire. Elle permet d'afficher ou non la recherche,
tout en conservant sa configuration.

- ``true`` permet d'afficher la recherche;
- ``false`` permet de masquer la recherche.

La clé ``advanced`` esrt obligatoire (pour la recherche avancée). Elle permet de p
reciser que le formulaire de recherche est un formulaire de recherche avancée et
non simple. Cette clé doit contenir le tableau des champs configurés pour la
recherche (voir plus bas pour la configuration des champs).

La clé ``default_form`` est optionelle. Elle permet de choisir quel formulaire
de recherche est ouvert par defaut. La valeur ``advanced'`` permet d'afficher le
formulaire multi-critères. Les autres valeurs, ou si ``default_form`` n'est pas
configuré, affichent le formulaire mono-critère.

La clé ``absolute_object`` est obligatoire. Elle permet de specifier à
openMairie le nom du modèle l'objet recherché. Ce nom est celui du fichier dans
``obj/``, ici ``om_utilisateur.class.php`` (sans son extension).

Configuration des critères de recherche
=======================================

La recherche avancée ne fonctionnera pas tant que la liste des champs du
formulaire multi-critères n'aura pas été créée. Ces champs sont appelés ici des
critères de recherche.

Configuration simple
--------------------

Un critère de recherche est représenté par un tableau PHP contenant sa
configuration.

.. code-block:: php

   <?php

   $champs['identifiant_utilisateur'] =
       array('colonne' => 'om_utilisateur',
             'table' => 'om_utilisateur',
             'type' => 'text',
             'libelle' => _('Identifiant'),
             'taille' => 10,
             'max' => 8));

   ?>


La clé ``identifiant_utilisateur`` est le nom du champ HTML qui sera affiché
sur le formulaire.

La clé ``colonne`` est obligatoire. Elle contient le nom de la colonne de la
base de données qui sera interrogee si la variable ``$_POST`` contient la clé
``identifiant_utilisateur``.

La clé ``table``  est obligatoire. Elle contient le nom de la table de la base
de données qui sera interrogée si la variable ``$_POST`` contient la clé
``identifiant_utilisateur``.

La clé ``'type`` est obligatoire. Elle contient le type du champ HTML à
afficher. Cela peut être ``date``, ``text``, ``select``, ou tout autre méthode
de la classe ``formulaire``. Pour les champs de type ``select``, le nom du champ
HTML doit etre le meme que le nom de la colonne.

La clé ``libelle`` est obligatoire. Elle contient le libellé qui sera affiché à
côté du champ dans le formulaire de recherche.

La clé ``taille`` est optionnelle. Elle contient la taille du champ HTML
(attribut HTML ``size``).

La clé ``max`` est optionnelle. Elle contient la longueur maximale de la valeur
du champ HTML (attribut HTML ``maxlength``).

Une fois tous les critères de recherche configurés, il faudra simplement
vérifier que le tableau des critères est bien utilisé par l'option de type
``search``.

Exemple de formulaire pour le tableau du modèle ``om_utilisateur``:

.. code-block:: php

   <?php

   $champs = array();

   $champs['login'] = array(
       'table' => 'om_utilisateur',
       'colonne' => 'login',
       'type' => 'text',
       'libelle' => _('Login'));
   
   $champs['email'] = array(
       'table' => 'om_utilisateur',
       'colonne' => 'email',
       'type' => 'text',
       'libelle' => _('E-mail'));
   
   $champs['om_profil'] = array(
       'table' => 'om_utilisateur',
       'colonne' => 'om_profil',
       'type' => 'select',
       'libelle' => _('Profil'));

    $options[] =  array('type' => 'search',
                        'display' => true,
                        'advanced' => $champs,
                        'default_form' => 'advanced',
                        'absolute_object' => 'om_utilisateur');

   ?>

Configuration avancée
---------------------


