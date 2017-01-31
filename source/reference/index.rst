.. _reference:

###################
Manuel de référence
###################

Il est proposé ici de décrire le fonctionnement du framework openMairie.

"En programmation orientée objet un framework est typiquement composé de classes mères qui seront dérivées et étendues par héritage en fonction des besoins spécifiques à
chaque logiciel qui utilise le framework".

http://fr.wikipedia.org/wiki/Framework

.. image :: architecture.png

openMairie intègre de nombreux composants : 

- FPDF : http://fpdf.org/
- TCPDF : https://tcpdf.org/
- DBPEAR : http://pear.php.net/package/DB/redirected
- PHPMailer : https://github.com/PHPMailer/PHPMailer
- JQUERY : https://jquery.com/
- JQUERY-UI : http://jqueryui.com/
- OPENLAYERS : https://openlayers.org/
- TINYMCE : https://www.tinymce.com/

Le framework est composé de 2 classes principales : 

om_dbform est une classe abstraite surchargée par les objets métiers qui fait le lien entre la base de données et le formulaire

om_formulaire est la classe qui construit le formulaire en utilisant l'objet form. 

Les classes métiers sont générées par le générateur une fois la base de données créées et surchargent la classe abstraite om_dbform. 

Enfin, il est possible de surcharger les classes métiers par des classes customisées.


.. toctree::
   :numbered:
   
   arborescence.rst
   initialisation_base_de_donnees.rst
   parametrage.rst
   acces.rst
   dashboard.rst
   listing.rst
   formulaire.rst
   edition.rst
   import.rst
   reqmo.rst
   sig/index.rst
   layout.rst
   filestorage.rst

