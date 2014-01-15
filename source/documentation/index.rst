.. _documentation:

################
La documentation
################

.. warning::

   Cette rubrique est en cours de rédaction.



*********
Préambule
*********

Ce chapitre vise à regrouper les bonnes pratiques qui permettent à toute la 
communauté openMairie de travailler sur les mêmes bases et avec les mêmes 
références concernant les outils de rédaction et de publication de documentation
afin de faciliter la contribution et d'obtenir un rendu homogène.

Voici les postulats :

* chaque logiciel/application possède une seule documentation Sphinx
  qui contient un manuel d'utilisation et un guide technique biens séparés
* le code source de la documentation doit être déposé dans un dépôt GIT sur 
  github.com
* la génération de la documentation est gérée de manière automatique par 
  readthedocs.org
* docs.openmairie.org est l'unique interface d'accès pour toutes les 
  documentations
* un lien dans le footer de chaque application permet d'accéder à la version
  de l'application du manuel d'utilisateur sur le site docs.openmairie.org


******
Sphinx
******



Installer sphinx
================





**********************************
Structure type d'une documentation
**********************************



Le manuel de l'utilisateur
==========================


Le guide du développeur
=======================




**********
github.com
**********

Pour pouvoir gérer un projet sur ce site, il faut avoir un utilisateur
(le bouton "Sign up" de la page d'acucueil permet d'obtenir un compte 
utilisateur très facilement).

L'objectif d'utiliser ce site est donc de faciliter la contribution 
à la documentation grâce à l'éditeur en ligne et au système de "Pull 
Request" et de déclencher simplement la regénération des documentations
grâce au lien automatique avec readthedocs.org.


Créer un projet
===============

Public(s) concerné(s) : Administrateur de projet openMairie.

Il s'agit ici, de créer un nouveau repository sur github.com dans l'organisation 
openmairie. Le projet doit s'appeler openlogiciel-documentation (par exemple pour
le logiciel openElec "openelec-documentation" et pour le logiciel openRésultat 
"openresultat-documentation"). Ne rien faire d'autre dans ce repository.

Depuis le tableau de bord de github.com, un clic sur le bouton "+" puis 
"New repository" en haut à droite de l'écran, à côté du login de 
l'utilisateur, permet d'accéder au formulaire de création d'un dépôt. 

Voici les informations à saisir : 

* Owner : sélectionner "openmairie", pour une meilleure lisibilité du projet 
  tous les dépôts doivent être créés dans cette organisation.

* Repository name : le nom doit être logiciel-documentation sans accents sans
  espaces en minuscules (par exemple pour le logiciel openElec 
  "openelec-documentation" et pour le logiciel openRésultat 
  "openresultat-documentation").

* Description : pour que le dépôt sorte correctement dans les recherches,
  il faut saisir "Documentation Logiciel Sphinx" (par exemple pour openCimetière 
  "Documentation openCimetière Sphinx").

* Public ou Private : sélectionner "Public" puisque les projets openMairie
  sont publics.

* Initialize this repository with a README : ne pas cocher la case.

Puis il suffit de cliquer sur le bouton "Create repository".


Importer la documentation depuis un projet subversion de l'adullact
===================================================================

Public(s) concerné(s) : Administrateur de projet openMairie.

Les pré-requis sont :

* le projet doit déjà être créé sur github.com
* être dans un terminal pour saisir les commandes suivantes
* les commandes : svn, svn2git et git doivent être installées


Étape 0 : Modifier et définir les variables utilisées dans les commandes suivantes ::

    export OLDPRODUCTNAME="openfoncier"
    export NEWPRODUCTNAME="openads"
    export ADULLACTUSER="fmichon"


Étape 1 : Récupération des logs ::
    
    mkdir -p ${NEWPRODUCTNAME}-documentation/${NEWPRODUCTNAME}-documentation && cd ${NEWPRODUCTNAME}-documentation/${NEWPRODUCTNAME}-documentation
    svn log -q svn://scm.adullact.net/scmrepos/svn/${OLDPRODUCTNAME}/documentation > ../LOG
    cat ../LOG | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > ../authors-transform.txt
    cat ../authors-transform.txt


Étape 2 : MANUEL - Modification des auteurs ::

    => Il faut faire la correspondance entre les utilisateurs de l'adullact et ceux de github
    => exemple : fmichon = Florent Michon <fmichon@atreal.fr>
    => fmichon = login de l'addulact
    => Florent Michon = nom complet du contributeur
    => fmichon@atreal.fr = mail référencé dans github
    vim ../authors-transform.txt    


Étape 3 : Vérification de l'intervalle ::

    export REVS="$(tail -n2 ../LOG|head -n1|awk '{print $1}'|sed "s/r//"):$(head -n2 ../LOG|tail -n1|awk '{print $1}'|sed "s/r//")"
    echo $REVS


Étape 4 : Récupération de tous les commits (très long) ::

    svn2git --authors ../authors-transform.txt --revision $REVS -v svn://scm.adullact.net/scmrepos/svn/${OLDPRODUCTNAME}/documentation


Étape 5 : Import du code sur github ::

    git remote add origin git@github.com:openmairie/${NEWPRODUCTNAME}-documentation.git
    git push -u --all
    git push --tags


Étape 6 : Suppression de l'ancien dépôt de documentation sur l'adullact pour que personne ne committe dessus ::

    svn del -m "Déplacement de la documentation vers Github" svn+ssh://${ADULLACTUSER}@scm.adullact.net/scmrepos/svn/${OLDPRODUCTNAME}/documentation/trunk svn+ssh://${ADULLACTUSER}@scm.adullact.net/scmrepos/svn/${OLDPRODUCTNAME}/documentation/branches
    echo "Documentation déplacée vers https://github.com/openmairie/${NEWPRODUCTNAME}-documentation" > ../MOVED-TO-GITHUB.txt
    svn import -m "Déplacement de la documentation vers Github" ../MOVED-TO-GITHUB.txt svn+ssh://${ADULLACTUSER}@scm.adullact.net/scmrepos/svn/${OLDPRODUCTNAME}/documentation/MOVED-TO-GITHUB.txt



Faire l'import initial d'un projet sphinx
=========================================

Public(s) concerné(s) : Administrateur de projet openMairie.


Contribuer à une documentation
==============================

Public(s) concerné(s) : Contributeur membre du projet openMairie.



***************
readthedocs.org
***************

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


*******************
docs.openmairie.org
*******************

Ce site permet de lister l'ensemble des documentations du projet openMairie.
