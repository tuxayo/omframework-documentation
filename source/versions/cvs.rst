.. _cvs:

################################
Concurrent versions system (CVS)
################################

`Site officiel du projet CVS <http://www.nongnu.org/cvs/>`_


Il est noté dans ce chapître les commandes de bases de cvs à titre d'indicatif.
Les développeurs d openMairie souhaitent qs'orienter sur SVN qui est plus facile dans
la gestion des répertoires (notament la suppression) et dans la mise en place
de composants externes (externals)

voir outils/svn et outils/svn_utilisation



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
