################################
Concurrent versions system (CVS)
################################

`Site officiel du projet CVS <http://www.nongnu.org/cvs/>`_


Il est noté dans ce chapitre les commandes de bases de cvs à titre d'indicatif.
Les développeurs d openMairie souhaitent s'orienter sur SVN qui est plus facile dans
la gestion des répertoires (notament la suppression) et dans la mise en place
de composants externes (externals)


=============
forge > local
=============

update ::

    $ cvs up
    $ cvs up -A // depuis la branche principale
    $ cvs up -r branch_name // depuis une branche
    $ cvs up -r tag_name  // depuis un tag
    $ cvs up -d  //creer sur le poste local les nouveaux repertoires
    $ cvs up -C  //Annulation des modifications locales -> les fichiers remplacés sont sauvés en .#file.revision	

message ::

    A Ajout de nouveaux éléments à la version locale
    M éléments modifiés localement (par rapport à la version de CVS) ;
    ? éléments inconnus de CVS (non présents dans la version de CVS) ;
    U pour les éléments modifiés dans CVS (par rapport à la version locale) ;
    C pour les éléments différents entre les versions locale et CVS, et qui posent un conflit à règler manuellement.
    D fichier supprimés



=====================
local -> forge
=====================

commit ::

    $ cd <répertoire désiré> 
    $ cvs commit [-m "message explicite de la modification"] [<arborescence>]
    $ cvs ci -m "ajout des fichiers" // avec message
    $ cvs ci module_name // dans la branche principale
    $ cvs ci -d branch_name module_name // dans une branche
    $ cvs ci -r tag module_name // depuis un tag
    $ cvs ci -r 3.0 //changer la révision (doit être plus grand que tous les numéros existants)

Add ::

    $ cd <répertoire désiré> 
    $ cvs add [-m "message explicite de l'ajout"] <arborescence>
    $ cvs add file1 file2 ...
    cvs add /data/pgsql*  // * tous les fichiers du repertoire  ...
    L'ajout d'un fichier n'est possible que depuis le répertoire le contenant ;
    Les fichiers d'un répertoire non ajouté ne sont pas visibles par CVS (lors de l'affichage des différences entre les versions locale et CVS) ;
    L'ajout d'éléments est effectif dans CVSROOT uniquement après la mise à jour de la version de CVS.

rm ::

    Pour supprimer des éléments à la version locale, afin de les supprimer ensuite définitivement de la version CVS :
    $ cd <répertoire désiré> 
    $ cvs remove [-f] <arborescence>
     * -f : pour effacer le fichier avant de le supprimer. 

    Il faut d'abord supprimer le fichier du répertoire 		
    $ rm file1 file2 ...
    $ cvs rm file1 file2 ...
    $ cvs ci -m "suppression des fichiers
    Supprimer un répertoire n est pas possible directement, il faut aller le supprimer dans le serveur.

=====
local
=====

Suppression d'une copie locale ::

    // pour éviter d'effacer un checkout en oubliant nos modifications
    Pour effacer les sources présentes sur le poste local et faire confiance au repository :
    $ cd <répertoire désiré> 
    $ cvs release [-d] <arborescence>
    * -d : pour effacer définitivement l'arborescence. 
    $ cvs release -d module

status ::

    Pour obtenir le détail (statut) de la version locale d'une arborescence :
    $ [ cd <répertoire désiré> ]
    $ cvs status <arborescence>
    cvs st -> status

diff/log ::

    cvs diff
    cvs log <nomfichier>
    cvs diff <nomfichier>     [local /differentiel]
    cvs diff -r 1.5 -r 1.6 nomfichier [entre 2 versions]

======
export
======
    
exemple opencimetiere_facture

- se mettre dans le repertoire : openmairie_cimetiere_facture

- taguer la version ::

    $ cvs tag version_2_03 (commence par une lettre / pas de point)

- faire l export ::

    $ cvs export - r version_2_03 -d version/opencimetiere_facture_2.03 openmairie_cimetiere_facture
                               [      version   ]   [repertoire export ] [module]



tag
===

Les identifiants logiques (noms donnés à une version par un utilisateur) sont différents
des identifiants CVS (du type 1.1.2.1). La gestion d'identifiant (ou tag) d'une arborescence se fait ainsi ::

    $ [ cd <répertoire désiré> ]
    $ cvs tag [-R] [-d -r] <nom de l'identifiant> [<arborescence>]
    $ cvs tag tag_name // creer un tag
    * -R : commande appliquée récursivement sur les sous-répertoires ;
    * -d -r : suppression de l'identifiant existant.

Exportation (mêmes options que cvs check out)

Pour exporter les sources du projet en vue d'une livraison (pas de répertoires CVS dans
l'arborescence) ::

    $ cd <répertoire désiré> 
    $ cvs export (-r <nom du tag> | -D <date désirée>) <arborescence>
     
    $ cvs export

Les fichiers .cvsignore sont exportés et apparaissent dans l'arborescence contrairement aux répertoires CVS ;
Des problèmes apparaissent lors d'export de fichiers binaires sur plateformes hétérogènes. Par exemple,
un export sur PC transforme les retours chariots (\n -> \r \n) 

======
Import
======

requete cvs ::

    cvs -d :pserver:user@cvs.mpl.ird.fr:/projet login
           (1)     (2)      (3)          (4)      
            |       |        |            |
            |       |        |            +- Répertoire du
            |       |        |    SERVEUR contenant les sources
            |       |        |              (racine)
            |       |        |    
            |       |        |  
            |       |        +-------- adresse du SERVEUR CVS
            |       |
            |       +------ Votre login à vous sur le SERVEUR
            |
            +----- le type d'authentification

========
checkout
========

Pour récupérer les sources du projet en local ::

    $ cd <répertoire désiré> 
    $ cvs checkout <arborescence>


======
Divers
======

aide ::

    $ cvs -H nomdecommande

log des commit : Pour obtenir l'historique d'une arborescence ::

    $ [ cd <répertoire désiré> ]
    $ cvs log <arborescence>

L'historique affiche les différents identifiants (ou tag) et les différentes versions
de l'arborescence concernée sous CVS;

L'historique sur un répertoire affiche récursivement les historiques des fichiers le
composant.

.cvsignore

La présence de fichier(s) .cvsignore dans un répertoire permet de dire à CVS d'ignorer
certains types de fichiers ::

    $ cd <répertoire désiré>
    $ cat .cvsignore
    *.jpg *.htm




====================================================================================
Changer le système de gestion des version de CVS vers SVN sur la forge de l'adullact
====================================================================================


Le but de ce tutorial est de changer le système de gestion de version sur un
projet existant sur la forge de l'adullact.


Pré-requis
==========

* Le projet sur l'adullact en CVS <NOM_DU_PROJET>
* Le nom du module à récupérer <NOM_DU_MODULE>
* Les droits d'administration sur le projet <NOM_DU_DEVELOPPEUR>


Etape 1 - Récupérer le code du CVS
==================================

::

    cvs -d :pserver:anonymous@scm.adullact.net:/cvsroot/<NOM_DU_PROJET> login
    cvs -d :pserver:anonymous@scm.adullact.net:/cvsroot/<NOM_DU_PROJET> export -DNOW <NOM_DU_MODULE>

Important : si un mot de passe est demandé, un mot de passe vide fera l'affaire.

A cette étape, il est recommandé de faire une archive du dossier
'<NOM_DU_MODULE>' qui vient d'être exporté pour le sauvegarder.


Etape 2 - Changer le type de dépôt
==================================

En tant qu'administrateur, aller dans l'onglet 'Sources' puis cliquer sur le
lien 'Administration'. Choisir alors SVN puis cliquer sur le bouton 'Mettre à
jour'.

Il faut ensuite attendre (le temps d'attente est variable entre 30 minutes et
plusieurs heures) que le dépôt subversion soit activé.


Etape 3 - Créer la structure du dépôt
=====================================

Ici nous allons créer la structure standard d'un dépôt Subversion :

* trunk
* tags
* branches

::

    svn mkdir svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOM_DU_PROJET>/trunk svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOM_DU_PROJET>/tags svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOM_DU_PROJET>/branches -m "Création de la structure du dépôt Subversion"


Etape 4 - Importer le code sur le nouveau dépôt Subversion
==========================================================

Cas 1
-----

Si l'application doit être utilisée telle qu'elle a été récupérée sur le CVS,
alors nous allons l'importer directement dans le dossier 'trunk'. ::

    svn import <NOM_DU_MODULE> svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOM_DU_PROJET>/trunk -m "Import de la version de l'application anciennement sous CVS"


Cas 2
-----

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


