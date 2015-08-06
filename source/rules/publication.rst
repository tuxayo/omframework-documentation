###########
Publication
###########

================
La documentation
================

Lorsqu'il y a une nouvelle version de l'application et que la version majeure ou mineure est incrémentée, il faut ajouter une nouvelle version de la documentation aussi.

Voici la liste des étapes à reproduire :

Sur GitHub :

* ajouter une nouvelle branch en reprenant la version majeure et mineure pour la nommer ;
* dans le readme de la documentation, modifier les versions ;
* dans le fichier source/conf.py, modifier les variables project, copyright, version et release ;
* dans le fichier source/index.rst, modifier le titre de la documentation ;
* dans settings, modifier la branche par défaut pour mettre la nouvelle.

Sur readthedoc :

* dans le menu admin, puis version, changer la version par défaut ;
* désactiver la version stable et latest.

Depuis l'URL docs.openmairie.org faire un "refresh" pour mettre à jour la page de présentation des documentations : http://docs.openmairie.org/?refresh

Dans l'application, modifier le lien dans le fichier doc/index.php pour pointer vers la nouvelle URL.

