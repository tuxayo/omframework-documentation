.. _optimisation:

############
optimisation
############

===========
compresseur
===========

->enleve les blanc


===========================
version compresse openLayer
===========================

utiliser le compresseur yahoo qui eneleve les blancs

http://developer.yahoo.com/yui/compressor/

===========================
faire une version compresse
===========================

construire un OpenLayers.js compresse dans le repertoire build

$ cd buill
$ python build.py 

le fichier fait 800 ko au lieu de 3 Mo

compression lite

$ python build.py lite.cfg

le fichier fait 120 ko
regarder dans lite les fichiers inclus

Mettre le repertoire img en /img
Mettre le style : ex theme/style/defaut.css

===
svn
===

svn a mettre dans externals la version 

===========
sur serveur
===========

optimisation des tuiles : tylecache
