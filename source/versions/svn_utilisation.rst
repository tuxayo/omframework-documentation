.. _svn_utilisation:

###############
svn utilisation
###############

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


