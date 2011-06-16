.. _cvs2svn:

####################################################################################
Changer le système de gestion des version de CVS vers SVN sur la forge de l'adullact
####################################################################################


Le but de ce tutorial est de changer le système de gestion de version sur un
projet existant sur la forge de l'adullact.


**********
Pré-requis
**********

* Le projet sur l'adullact en CVS <NOM_DU_PROJET>
* Le nom du module à récupérer <NOM_DU_MODULE>
* Les droits d'administration sur le projet <NOM_DU_DEVELOPPEUR>


**********************************
Etape 1 - Récupérer le code du CVS
**********************************

::

    cvs -d :pserver:anonymous@scm.adullact.net:/cvsroot/<NOM_DU_PROJET> login
    cvs -d :pserver:anonymous@scm.adullact.net:/cvsroot/<NOM_DU_PROJET> export -DNOW <NOM_DU_MODULE>

Important : si un mot de passe est demandé, un mot de passe vide fera l'affaire.

A cette étape, il est recommandé de faire une archive du dossier
'<NOM_DU_MODULE>' qui vient d'être exporté pour le sauvegarder.


**********************************
Etape 2 - Changer le type de dépôt
**********************************

En tant qu'administrateur, aller dans l'onglet 'Sources' puis cliquer sur le
lien 'Administration'. Choisir alors SVN puis cliquer sur le bouton 'Mettre à
jour'.

Il faut ensuite attendre (le temps d'attente est variable entre 30 minutes et
plusieurs heures) que le dépôt subversion soit activé.


*************************************
Etape 3 - Créer la structure du dépôt
*************************************

Ici nous allons créer la structure standard d'un dépôt Subversion :

* trunk
* tags
* branches

::

    svn mkdir svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOM_DU_PROJET>/trunk svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOM_DU_PROJET>/tags svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOM_DU_PROJET>/branches -m "Création de la structure du dépôt Subversion"


**********************************************************
Etape 4 - Importer le code sur le nouveau dépôt Subversion
**********************************************************

=====
Cas 1
=====

Si l'application doit être utilisée telle qu'elle a été récupérée sur le CVS,
alors nous allons l'importer directement dans le dossier 'trunk'. ::

    svn import <NOM_DU_MODULE> svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOM_DU_PROJET>/trunk -m "Import de la version de l'application anciennement sous CVS"


=====
Cas 2
=====

Dans le cas de figure où l'application va être migrée vers OM4, nous allons
placer le code récupéré sur le CVS dans une branche du dépôt correspondant à sa
version. ::

    svn import <NOM_DU_MODULE> svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOM_DU_PROJET>/branches/1.x -m "Import de la version de l'application anciennement sous CVS"


Dans ce cas, il faut donc importer le nouveau code dans le dossier 'trunk' :

* Soit le développement du projet n'a pas encore commencé et il suffit de
  suivre le tutorial "versions/svn/Importer un nouveau projet".

* Soit le développement du projet a déjà commencé et il suffit d'importer le
  dossier en cours de développement (attention : il ne faut pas que des
  dossiers .svn soient présents dans ce dossier et il faut prendre soin de
  supprimer les dossiers récupérés depuis les EXTERNALS avant l'import) : ::

    svn import <NOM_DU_DOSSIER> svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOM_DU_PROJET>/trunk -m "Import initial de l'application"

  Il faut bien sur valider les EXTERNALS pour récupérer les librairies externes.
