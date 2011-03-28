.. _svn_utilisation:

###############
svn utilisation
###############

il est propose dans ce chapitre de lister des commandes testees en svn

==============
commande linux 
==============

pour ne pas recopier les commandes ::

    $ history       // historque des 200 commandes   
    $!10 	        //lance la commande 10


============
Commande SVN
============

creer un projet ::

    svn mkdir svn+ssh://fraynaud@scm.adullact.net/svnroot/openrecensement/trunk
    svn import . svn+ssh://fraynaud@scm.adullact.net/svnroot/openrecensement/trunk  --> pas de lien svn

creation dans le repertoire et recuperer les fichiers ::


    exemple du courrier ::
    /var/www/openmairie_courrier3$ svn co svn+ssh://fraynaud@scm.adullact.net/svnroot/opencourrier/tags
    /var/www/openmairie_courrier3$ svn co svn+ssh://fraynaud@scm.adullact.net/svnroot/opencourrier/branches
    /var/www/openmairie_courrier3$ svn co svn+ssh://fraynaud@scm.adullact.net/svnroot/opencourrier/trunk

    exemple du foncier ::
    /var/www/openmairie_foncier$ svn co svn+ssh://fraynaud@scm.adullact.net/svnroot/openfoncier/branches
    /var/www/openmairie_foncier$ svn co svn+ssh://fraynaud@scm.adullact.net/svnroot/openfoncier/tags
    /var/www/openmairie_foncier$ svn co svn+ssh://fraynaud@scm.adullact.net/svnroot/openfoncier/trunk

Creation du repertoire sur cible + fichiers ::

    avec co

    /var/www$ svn co svn+ssh://fraynaud@scm.adullact.net/svnroot/openelec svn.adullact.openelec
    /var/www$ svn co https://svn.atreal.net/opencollectivites/openmairie_exemple/trunk openmairie_exemple_atreal

    avec checkout

    /var/www$ svn checkout svn+ssh://fraynaud@scm.adullact.net/svnroot/openrecensement/trunk openmairie_recensement
    
    svn checkout svn+ssh://nom-du-développeur@scm.adullact.net/svnroot/openodp

taguer une version tag 2.00 (n importe ou) ::

    /var/www$ svn cp svn+ssh://fraynaud@scm.adullact.net/svnroot/openelec/trunk svn+ssh://fraynaud@scm.adullact.net/svnroot/openelec/tags/2.00
    /var/www$ svn cp svn+ssh://fraynaud@scm.adullact.net/svnroot/openmairie/openmairie/trunk svn+ssh://fraynaud@scm.adullact.net/svnroot/openmairie/openmairie/tags/4.00beta

export creation de version ( a voir la creation dans versions) === en  var/www ::

    svn export svn+ssh://fraynaud@scm.adullact.net/svnroot/openelec/tags/2.00  version/openelec_2.00
    svn export svn+ssh://fraynaud@scm.adullact.net/svnroot/openrecensement/trunk  var/www/versions/openrecensement_1.00beta
    svn export svn+ssh://fraynaud@scm.adullact.net/svnroot/openmairie/openmairie_exemple/tags/4.0.0beta  versions/openmairie_exemple_4.0.0beta 
    svn export svn+ssh://fraynaud@scm.adullact.net/svnroot/openmairie/openmairie/tags/4.00beta versions/openmairie_4.0.0beta 
                          


update dans le repertoire ::

    svn st 			// verification fichier sur client / adullact.net
    svn up 			// maj depuis adullact.net

    A Ajout de nouveaux éléments à la version locale
    M éléments modifiés localement (par rapport à la version de SVN) ;
    ? éléments inconnus de SVN (non présents dans la version de SVN) ;
    U pour les éléments modifiés dans SVN (par rapport à la version locale) ;
    C pour les éléments différents entre les versions locale et SVN, et qui posent un conflit à règler manuellement.
    ?D fichier supprimés (a verifier)

commit dans le repertoire::

    svn add nomfichier 	// ajout au repository (ne commite pas)
    svn del nomfichier 	// supprime le fichier sur le client et le supprimme au prochain commit
    svn ci   validation de fichier modifies client

    svn revert nomfichier 			// remet dans le dernier etat du svn (soit pas del soit pas modifier)
    svn diff nomdossier ou nomfichier 	// affiche les modifications r/r au dernier svn up
    svn ci nomfichier ou nomdossier 	

resolution de conflit ::

    svn st
    !     C openmairie_exemple/trunk/authors.txt
          >   local édition, suppression entrante sur mis à jour
    
    svn revert openmairie_exemple/trunk/authors.txt
        'openmairie_exemple/trunk/authors.txt' réinitialisé

deplacer un dossier sur le svn -> commande mv ::

Exemple : on a créé trunk/trunk/dossiers_source

D'abord on renomme le premier dossier trunk en dossier branches
> svn mv svn+ssh://fraynaud@scm.adullact.net/svnroot/openboisson/trunk svn+ssh://fraynaud@scm.adullact.net/svnroot/openboisson/branches
cela fait branches/trunk/dossiers_souce

Ensuite on déplace le dossier trunk qui se trouve maintenant dans branches à la racine du dépôt
> svn mv svn+ssh://fraynaud@scm.adullact.net/svnroot/openboisson/branches/trunk svn+ssh://fraynaud@scm.adullact.net/svnroot/openboisson/trunk
cela fait trunk/dossiers_source

	
meld : comparer les versions ::
===============================

    meld openmairie_recensement/ svn.openmairtrunck/trun