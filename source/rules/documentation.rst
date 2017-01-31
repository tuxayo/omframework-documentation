#############
Documentation
#############

Ce paragraphe vise à regrouper les bonnes pratiques qui permettent à toute la 
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

*************************
Quelques bonnes pratiques
*************************

Les blocs de code PHP
---------------------

Lorsqu'un exemple de code PHP est présenté dans la documentation, afin d'obtenir une coloration syntaxique plus claire à la lecture, il doit être déclaré de la manière suivante ::

    .. code-block:: php

      <?php
      ...
      ?>

