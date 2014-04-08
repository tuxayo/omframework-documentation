###############
readthedocs.org
###############

readthedocs.org est un site qui héberge de la documentation, la rendant
accessible et facile à trouver. Il est possible d'importer les
documentations sur ce site depuis les système de gestion de version tel
que Subversion, Git ou d'autres. Ce site permet de gérer la mise à jour
automatique des documentations à chaque commit dans ces systèmes de gestion
de version. Le site supporte également le support des versions mais seulement
pour Git et non pas pour Subversion à l'heure où cette documentation est
rédigée.

L'objectif d'utiliser ce site est donc de ne pas avoir à se soucier de la 
génération des documentations. C'est ReadTheDcs.org qui s'en occupe et 
dans tous les formats html, pdf, epub, ... 

Pour pouvoir gérer un projet sur ce site, il faut avoir un utilisateur
(le bouton "Inscription" en haut à droite de la page d'acucueil permet 
d'obtenir un compte utilisateur très facilement).


==================================
Importer un nouveau projet sur RTD
==================================

Public(s) concerné(s) : Administrateur de projet openMairie.

Depuis le tableau de bord de readthedocs.org, un clic sur le bouton 
"Importer", permet d'accéder au formulaire de création d'un projet 
sphinx existant.

Voici les informations à saisir : 

* Nom : le nom du logiciel sans accents sans espaces en minuscules (par exemple : 
  openelec ou openresultat).

* Repo : l'URL de github.com où est stockée le code de la documentation (par 
  exemple: https://github.com/openmairie/openelec-documentation.git pour openelec
  ou https://github.com/openmairie/openresultat-documentation.git pour openresultat).

* Type de dépôt : "Git" puisque le dépôt est sur github.com.

* Description : Le nom du logiciel avec accents avec espaces et avec la casse
  (par exemple : openElec ou openRésultat).

* Language : "French" puisque la documentation est francophone.

* URL Projet : "http://www.openmairie.org/".

* Canonical URL : laissons vide pour le moment.

* Single version : ne pas cocher la case.

* Etiquettes : "openmairie".

Puis il suffit de cliquer sur le bouton "Créer". 

Si la création du projet s'est bien passée une version de la documentation a 
du être générée, celle-ci est disponible en cliquant sur le bouton 
"Afficher les docs" sur la page du projet nouvellement créé.


====================================================
Paramétrer une nouvelle version d'un projet existant
====================================================

Public(s) concerné(s) : Administrateur de projet openMairie.

Par défaut un projet sur readthedocs.org gère uniquement la dernière version de
la cocumentation 'latest' en récupérant la branche par défaut de la documentation
sur github.com 'master'.

Il est possible de gérer plusieurs versions de la documentation pour obtenir des 
URL du style : 

* http://omframework.readthedocs.org/fr/4.2/
* http://omframework.readthedocs.org/fr/4.4/
* http://omframework.readthedocs.org/fr/latest/

Chaque version dans readthedocs.org, correspond à une branche dans le dépôt du 
projet sur github.com.

...


