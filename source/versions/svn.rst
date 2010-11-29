.. _svn:

#######################
Apache Subversion (SVN)
#######################

`Site officiel du projet SVN <http://subversion.apache.org/>`_

**********
Pré-requis
**********

Installer subversion :
`<http://subversion.apache.org/packages.html>`_  


**************
L'arborescence
**************

Voici l'arborescence standard d'un projet versionné sur un SVN : ::

    /trunk/
    /tags/
    /branches/


* trunk/ : la version en cours de développement.

* tags/ : les différentes versions publiées. Les dossiers dans tags/ sont des
  copies du dossier trunk/ a un instant précis. Ils permettent de fixer une
  version pour la publier. Il est interdit d'effectuer une modification dans un
  de ces dossiers, la bonne méthode étant de faire la modification dans le
  trunk/ et de faire une nouvelle version dans le dossier tags/.

* branches/ : ...



***************
Les règles d'or
***************

* Ne jamais commiter dans un tag.
* Ne jamais commiter sans message de commit.
* Ne jamais tagger une version qui contient des externals vers un 'trunk'.


**********************************
Les commandes basiques à connaître
**********************************


Récupérer une copie locale : ::

    svn co svn+ssh://nom-du-développeur@scm.adullact.net/openmairie/openmairie_exemple/trunk openmairie_exemple
    

Mettre à jour sa copie locale : ::

    svn up


Voir l'état de sa copie locale : ::

    svn st

Voir la différence entre sa copie locale et le dépôt : ::

    svn diff

::

    svn ci


*********
Externals
*********

C'est une propriété sur le dépôt SVN permettant d'importer du code provenant
d'un dépôt différent.

Le fichier EXTERNALS.txt : ::

    #
    # created by: svn propset svn:externals -F ./EXTERNALS.txt .
    #
    
    openmairie svn://scm.adullact.net/svnroot/openmairie/openmairie/tags/4.0.0/
    fpdf svn://scm.adullact.net/svnroot/openmairie/externals/fpdf/tags/1.6-min/
    pear http://svn.php.net/repository/pear/pear-core/tags/PEAR-1.9.1/
    db http://svn.php.net/repository/pear/packages/DB/tags/RELEASE_1_7_13/


Appliquer les propriétés externals : ::

    svn propset svn:externals -F ./EXTERNALS.txt .
    svn up
    svn ci


********
Keywords
********


**********************
Les clients graphiques
**********************

Il est recommandé de savoir utiliser et d'utiliser subversion en ligne de
commande mais il existe quelques clients graphiques qui permettent de réaliser
certaines opérations d'une manière plus conviviale.

* Meld
* TortoiseSVN
* ...


*********
Tutoriaux
*********

==========================
Importer un nouveau projet
==========================

Un nouveau projet est une nouvelle application qui se base sur la dernière
version taggée d'openmairie_exemple. Ce tutorial contient certains pré-requis
comme la création du projet sur la forge, le fait d'avoir un utilisateur avec
les droits corrects sur le projet, le fait d'avoir consulter la `dernière
version taggée d'openmairie_exemple <https://adullact.net/scm/viewvc.php/openmairie_exemple/tags/?root=openmairie>`_

On se positionne dans la dossier tmp pour récupérer la dernière version
d'openmairie_exemple ::

    cd /tmp
    svn export --ignore-externals svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/<DERNIERE_VERSION_OPENMAIRIE_EXEMPLE>/ openexemple

On cré l'arborescence standard sur le dépôt ::

    svn mkdir svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOUVEAU_PROJET>/trunk
    svn mkdir svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOUVEAU_PROJET>/tags
    svn mkdir svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOUVEAU_PROJET>/branches

On se positionne dans le dossier précédemment importé pour importer sur le
dépôt son contenu ::

    cd openexemple
    svn import . svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOUVEAU_PROJET>/trunk

On se positionne dans son dossier de développement pour créer la copie
locale du projet ::
    
    cd ~/public_html/
    svn co svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOUVEAU_PROJET>/trunk <NOUVEAU_PROJET>

On se positionne dans le dossier php de l'application pour appliquer
les externals ::
    
    cd <NOUVEAU_PROJET>/php
    svn propset svn:externals -F ./EXTERNALS.txt .
    svn up
    svn ci


============================
Publier une nouvelle version
============================

Ce tutorial contient certains pré-requis comme le fait d'avoir un utilisateur
avec les droits corrects sur le projet ou connaître comment incrémenter le
numéro de version de l'application à publier.

Ici on fait une copie du 'trunk' vers le dossier 'tags' de l'application
openmairie_exemple ::

    svn cp svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/openmairie/openmairie_exemple/trunk svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/openmairie/openmairie_exemple/tags/<NOUVELLE_VERSION>

