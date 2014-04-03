#######################
Apache Subversion (SVN)
#######################

`Site officiel du projet SVN <http://subversion.apache.org/>`_

**********
Pré-requis
**********

Installer subversion :
`<http://subversion.apache.org/packages.html>`_  

Il existe également des outils graphiques comme TortoiseSVN (Windows).


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

    svn co svn+ssh://nom-du-développeur@scm.adullact.net/openmairie/openmairie_exemple/trunk
        openmairie_exemple
    

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
    
    core  svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/core/
    spg   svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/spg/
    scr   svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/scr/
    lib   svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/lib/
    css   svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/css/
    js    svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/js/
    img   svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/img/
    pdf   svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/pdf/
    php   svn://scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.2.0/php/
    
    om-theme svn://scm.adullact.net/svnroot/openmairie/externals/om-theme/kinosura/tags/1.0.0


Appliquer les propriétés externals : ::

    svn propset svn:externals -F ./EXTERNALS.txt .
    svn up
    svn ci

attention le repertoire ne doit pas etre existant (copie de travail verouillée)


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
    svn export --ignore-externals svn://scm.adullact.net/svnroot/openmairie/
        openmairie_exemple/tags/<DERNIERE_VERSION_OPENMAIRIE_EXEMPLE>/ openexemple

On cré l'arborescence standard sur le dépôt ::

    svn mkdir svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/scmrepos/svn/<NOUVEAU_PROJET>/trunk
    svn mkdir svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/scmrepos/svn/<NOUVEAU_PROJET>/tags
    svn mkdir svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/scmrepos/svn/<NOUVEAU_PROJET>/branches

On se positionne dans le dossier précédemment importé pour importer sur le
dépôt son contenu ::

    cd openexemple
    svn import . svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/svnroot/<NOUVEAU_PROJET>/trunk

On se positionne dans son dossier de développement pour créer la copie
locale du projet ::
    
    cd ~/public_html/
    svn co svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/scmrepos/svn/<NOUVEAU_PROJET>/trunk
            <NOUVEAU_PROJET>

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

Avant de publier une application, il faut vérifier que l'EXTERNALS de la
librairie openMairie ne pointe pas vers le 'trunk'. Pour cela ::

    less php/EXTERNALS.txt
    
    #
    # created by: svn propset svn:externals -F ./EXTERNALS.txt .
    #
    
    openmairie svn://scm.adullact.net/svnroot/openmairie/openmairie/trunk/
    fpdf svn://scm.adullact.net/svnroot/openmairie/externals/fpdf/tags/1.6-min/
    pear http://svn.php.net/repository/pear/pear-core/tags/PEAR-1.9.1/
    db http://svn.php.net/repository/pear/packages/DB/tags/RELEASE_1_7_13/

Ici on voit que openmairie pointe vers le 'trunk'. Nous devons d'abord publier
la librairie ::

   svn cp svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/openmairie/openmairie/trunk
          svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/openmairie/openmairie/tags/<NOUVELLE_VERSION>

Le message pourra être : Tag openmairie <NOUVELLE_VERSION>.

Ensuite il faut changer les EXTERNALS.txt. On remplace dans le fichier
php/EXTERNALS.txt, le trunk par la nouvelle version ::

    vim php/EXTERNALS.txt
    
    #
    # created by: svn propset svn:externals -F ./EXTERNALS.txt .
    #
    
    openmairie svn://scm.adullact.net/svnroot/openmairie/openmairie/tags/<NOUVELLE_VERSION>/
    fpdf svn://scm.adullact.net/svnroot/openmairie/externals/fpdf/tags/1.6-min/
    pear http://svn.php.net/repository/pear/pear-core/tags/PEAR-1.9.1/
    db http://svn.php.net/repository/pear/packages/DB/tags/RELEASE_1_7_13/    

Ensuite on applique le nouveau propset externals une fois placé dans le dossier
php (Attention de ne pas oublier le "." dans la commande svn propset) ::

    cd php/
    svn propset svn:externals -F ./EXTERNALS.txt .
    svn up

Ici en faisant un svn info sur le dossier openmairie, nous devons obtenir une
URL comme ceci ::
    
    svn info openmairie/
    URL : svn://scm.adullact.net/svnroot/openmairie/openmairie/tags/<NOUVELLE_VERSION>
    
Si tout est ok nous pouvons valider nos modifications puis passer à la
publication de l'application ::

    svn ci

Ici on fait une copie du 'trunk' vers le dossier 'tags' de l'application
openmairie_exemple ::

    svn cp svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/openmairie/openmairie_exemple/trunk
           svn+ssh://<NOM_DU_DEVELOPPEUR>@scm.adullact.net/openmairie/openmairie_exemple/tags/<NOUVELLE_VERSION>


===============
svn utilisation
===============

il est propose dans ce chapitre de lister quelques  commandes utiles
en cas de conflit testées en svn

type de fichier ::

    A Ajout de nouveaux éléments à la version locale
    M éléments modifiés localement (par rapport à la version de SVN) ;
    ? éléments inconnus de SVN (non présents dans la version de SVN) ;
    U pour les éléments modifiés dans SVN (par rapport à la version locale) ;
    C pour les éléments différents entre les versions locale et SVN, et qui posent un conflit à règler manuellement.
    ?D fichier supprimés (a verifier)

revert et diff::

    svn revert nomfichier 			// remet dans le dernier etat du svn (soit pas del soit pas modifier)
    svn diff nomdossier ou nomfichier 	// affiche les modifications r/r au dernier svn up
	

resolution de conflit ::

    svn st
    !     C openmairie_exemple/trunk/authors.txt
          >   local édition, suppression entrante sur mis à jour
    
    svn revert openmairie_exemple/trunk/authors.txt
        'openmairie_exemple/trunk/authors.txt' réinitialisé

deplacer un dossier sur le svn -> commande mv ::

    Exemple : on a créé trunk/trunk/dossiers_source
    
    D'abord on renomme le premier dossier trunk en dossier branches
    > svn mv svn+ssh://fraynaud@scm.adullact.net/scmrepos/svn/openboisson/trunk
        svn+ssh://fraynaud@scm.adullact.net/scmrepos/svn/openboisson/branches
    cela fait branches/trunk/dossiers_souce
    
    Ensuite on déplace le dossier trunk qui se trouve maintenant dans branches à la racine du dépôt
    > svn mv svn+ssh://fraynaud@scm.adullact.net/scmrepos/svn/openboisson/branches/trunk
        svn+ssh://fraynaud@scm.adullact.net/scmrepos/svn/openboisson/trunk
    

    cela fait trunk/dossiers_source

creer - deplacer (autre exemple) - detruire un repertoire sur svn ::

    creer un dossier documentation sur svn depuis une copie loacle
    svn import documentation svn+ssh://fraynaud@scm.adullact.net/scmrepos/svn/opencimetiere/documentation

    renomer = renommer sur le svn trunk en temp
    svn rename svn+ssh://fraynaud@scm.adullact.net/scmrepos/svn/opencimetiere/documentation/trunk
               svn+ssh://fraynaud@scm.adullact.net/scmrepos/svn/opencimetiere/documentation/temp

    move = deplacer le dossier temp/trunk vers trunk
    svn mv svn+ssh://fraynaud@scm.adullact.net/scmrepos/svn/opencimetiere/documentation/temp/trunk
           svn+ssh://fraynaud@scm.adullact.net/scmrepos/svn/opencimetiere/documentation/trunk

    delete  detruire le repertoire temp
    svn del svn+ssh://fraynaud@scm.adullact.net/scmrepos/svn/opencimetiere/documentation/temp


Creation d une nouvelle version ::

    Copie en tag de la version

    svn cp  svn+ssh://fraynaud@scm.adullact.net/scmrepos/svn/openboisson/trunk
            svn+ssh://fraynaud@scm.adullact.net/scmrepos/svn/openboisson/tags/1.0.0beta

    export dans un repertoire local openmairie_debitboisson_1.0.0beta sans les repertoires .svn

    svn export  svn+ssh://fraynaud@scm.adullact.net/scmrepos/svn/openboisson/tags/1.0.0beta
            openmairie_debitboisson_1.0.0beta

