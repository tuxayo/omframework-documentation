.. _filestorage:

####################################################
Abstraction du 'filestorage' (stockage des fichiers)
####################################################

.. contents::

================
Principe général
================

L'objectif de cette rubrique est de gérer le stockage des fichiers dans les
applications. Ce stockage permet d'avoir des stockages complexes.

Termes :

* **abstracteur** : classe d'abstraction, c'est elle qui est instanciée et qui
  instancie les classes spécialisées en fonction des critères,
* **connecteur** : classe spécialisée.


Description des fichiers permettant de gérer le filestorage : ::

    [D] dyn
     |-[F] filestorage.inc.php
     |---- ...
    [D] core
     |-[F] om_filestorage.class.php
     |-[F] om_filestorage_deprecated.class.php
     |-[F] om_filestorage_filesystem.class.php
     |-[F] om_filestorage_filetransferprotocol.class.php *
     |-[F] om_filestorage_alfresco.class.php *
     |---- ...
    
    * (ce fichier n'existe pas il est là à titre d'illustration de ce que le système est capable de faire)




==============
Fonctionnement
==============

Description de l'abstracteur
----------------------------

[core/om_filestorage.class.php]

Ce fichier est composé de deux classes : la classe d'abstraction, la classe de
base pour les classes spécialisées.

La classe 'filestorage' est une classe d'abstraction de stockage de fichiers.
C'est cette classe qui est instanciée et utilisée par d'autres scripts pour
gérer la création, récupération, suppression de fichiers et ce peu importe le
stockage utilisé. Son objectif est d'instancier la classe de stockage spécifique
aussi appelée plugin de stockage correspondant au paramétrage sélectionné. Cette
classe de stockage spécifique hérite de la classe 'filestorage_base' qui lui sert
de modèle.


Description du fichier de configuration
---------------------------------------

[dyn/filestorage.inc.php]

Ce fichier de configuration doit permettre le paramétrage du stockage dans
chacune des applications. Par exemple, il permet dans une installation
d'openElec de stocker les fichiers en utilisant le connecteur 'filesystem' dans
le path /var/openelec/data sur le système de fichiers et dans une autre
installation d'openElec de stocker les fichiers dans 'alfresco' sur un autre
serveur.

Il doit permettre de sélectionner le stockage à utiliser ainsi que le
paramétrage spécifique à chacun des connecteurs.

Par exemple : pour le stockage 'filesystem', il doit être possible de paramétrer
le répertoire dans lequel vont être stockés les fichiers, pour le stockage
'alfresco' il doit être possible de paramétrer l'adresse de l’hôte,
l'identifiant et le mot de passe de connexion, ....

Il permet aussi de stocker la configuration du stockage de fichier temporaire.

Ce fichier s'inspire des autres fichiers de configuration (mail.inc.php,
directory.inc.php, ...). Il doit contenir un tableau associatif : ::

    $filestorage = array();

Exemple d'un paramétrage (la clé filestorage-default doit être rajoutée dans
une des valeurs du paramétrage du fichier database.inc.php comme l'est
mail-default ou directory-default) : ::

    $filestorage["filestorage-default"] = array {
        "storage" => "filesystem", // l'attribut storage est obligatoire
        "storage_path" => "", // l'attribut storage_path n'est pas obligatoire
        "path" => "/var/www/openfoncier/data/",
        "temporary" => array(
            "storage" => "filesystem", // l'attribut storage est obligatoire
            "path" => "../tmp/", // le repertoire de stockage
        ...
    }

Si aucun filestorage n'est paramétré, le filestorage deprecated sera instancié et
le filestorage temporaire sera le filesystem.

Description des méthodes de la classe filestorage
-------------------------------------------------

La classe 'filestorage' contient des méthodes de gestion des fichiers :

.. method:: filestorage.create($data, $metadonnees, $mode = "from_content")

   Permet de créer un fichier sur le filestorage,

   $data : contenu du fichier

   $metadonnees : tableau contenant la liste des métadonnées ($cle => valeur)

   $mode ["from_content", "from_path"] :

   - from_content : $data contient le contenu du fichier.

   - from_temporary : $data l'uid d'un fichier enregistré sur le filesystem temporary.

   - from_path : $data contient le chemin du fichier à enregistrer.

   Cette méthode retourne l'UUID du fichier enregistré.

.. method:: filestorage.update($uid, $data, $metadonnees, $mode = "from_content")

   Permet de mettre à jour un fichier sur le filestorage,

   $data : contenu du fichier

   $metadonnees : tableau contenant la liste des métadonnées ($cle => valeur)

   $mode ["from_content", "from_path"] :

   - from_content : $data contient le contenu du fichier.

   - from_temporary : $data l'uid d'un fichier enregistré sur le filesystem temporary.

   - from_path : $data contient le chemin du fichier à enregistrer.

   Cette méthode retourne l'UUID du fichier enregistré.

.. method:: filestorage.get($uid)

    Cette méthode retourne le contenu et les métadonnées d'un fichier en fonction
    de l'UUID passé en paramètre.

.. method:: filestorage.delete($uid)

    Cette méthode supprime un fichier en fonction de l'UUID passé en paramètre.

.. method:: filestorage.create_temporary($data, $metadonnees, $mode = "from_content")

   Permet de créer un fichier sur le filestorage temporaire,

    $data : contenu du fichier

   $metadonnees : tableau contenant la liste des métadonnées ($cle => valeur)

   $mode ["from_content", "from_path"] :

   - from_content : utilisation normale de la méthode create(), $data contient
     le contenu du fichier.

   - from_path : $data contient le chemin du fichier à enregistrer.

   Cette méthode retourne l'UUID du fichier enregistré temporairement.

.. method:: filestorage.get_temporary($uid)

    Cette méthode retourne le contenu et les métadonnées d'un fichier enregistré
    temporairement en fonction de l'UUID passé en paramètre.

.. method:: filestorage.delete_temporary($uid)

    Cette méthode supprime un fichier temporaire en fonction de l'UUID passé en paramètre.


L'appel aux méthodes "temporary" se fait sur une instance de filesystem défini
dans le paramétrage.
Ces méthodes sont implémentés dans la classe de base contrairement aux autres
méthodes, elle peuvent toutefois être surchargées dans les classes de connecteurs
spécifiques.


Description du connecteur **depredacted**
-----------------------------------------

[core/om_filestorage_deprecated.class.php]

Cette classe est une classe de stockage spécifique aussi appelée plugin de
stockage pour le système d'abstraction de stockage des fichiers. Le principe de
ce plugin est de stocker tous les fichiers à plat selon la méthode utilisée
avant la création du système de stockage. Ce plugin a été créé uniquement dans
un soucis de garder la compatibilité pour les applications existantes.



Description du connecteur **filesystem**
----------------------------------------

[core/om_filestorage_filesystem.class.php]

Cette classe est une classe de stockage spécifique aussi appelée plugin de
stockage pour le système d'abstraction de stockage des fichiers. Le principe de
ce plugin est de stocker tous les fichiers en renommant le fichier avec un UUID
(identifiant unique) et en créant une arborescence à deux niveaux. Le premier
est composé des deux premiers caractères de l'UUID du fichier et le second
niveau des quatre premiers caractères de l'UUID du fichier. Un fichier avec
l'extension .info permet de stocker les informations de base du fichier ainsi
que des métadonnées.

Schéma du stockage : ::

    /repertoire/de/stockage/25/252e/252ece72d4c0f88782d9fd6b99f43dfd

    /repertoire/de/stockage/ :
    /25/ : Le premier niveau des dossiers contenant les deux premiers caractères de l'uuid du fichier, la méthode de génération des uuid est fourni dans la suite du paragraphe
    /252e/ : Le second niveau des dossiers contenant les 4 premiers caractères de l'uuid du fichier, la méthode de génération des uuid est fourni dans la suite du paragraphe
    252ece72d4c0f88782d9fd6b99f43dfd : Le fichier est stocké avec pour nom un uuid sans extension, la méthode de génération des uuid est fourni dans la suite du paragraphe

    252ece72d4c0f88782d9fd6b99f43dfd.info : Les fichiers .info sont là pour stocker les métadonnées de chaque fichier, ce sont des fichiers textes qui sont formatés :

    # trois informations obligatoires (ces commentaires ne doivent pas apparaître dans le fichier .info)

    filename="plop.pdf"
    mimetype="application/pdf"
    size="124541"

    # métadonnées supplémentaires facultatives (ces commentaires ne doivent pas apparaître dans le fichier .info)

    propriete1="valeur1"
    propriete2="valeur2"

    252ece72d4c0f88782d9fd6b99f43dfd_lock : Les fichiers _lock sont là pour servir de marqueur et indiquer si le fichier est locké ou non.

Exemple d'arborescence de stockage : ::

    /repertoire/de/stockage/25/252e/252ece72d4c0f88782d9fd6b99f43dfd
    /repertoire/de/stockage/25/252e/252ece72d4c0f88782d9fd6b99f43dfd.info
    /repertoire/de/stockage/25/252e/252ece72d4c0f88782d9fd6b99f43dfd_lock
    /repertoire/de/stockage/25/252e/252eacd35ef547dab12ded6b99f43dfd
    /repertoire/de/stockage/25/252e/252eacd35ef547dab12ded6b99f43dfd.info
    /repertoire/de/stockage/25/252e/252eacd35ef547dab12ded6b99f43dfd_lock
    /repertoire/de/stockage/12/123a/123aacd35ef547dab12ded6b99f43dfd
    /repertoire/de/stockage/12/123a/123aacd35ef547dab12ded6b99f43dfd.info
    /repertoire/de/stockage/12/123a/123aacd35ef547dab12ded6b99f43dfd_lock

Méthode pour générer les uuid : ::

    function generate_uuid($prefix = "") {
        return md5(uniqid($prefix, true));
    }




Description du connecteur **filetransferprotocol**
--------------------------------------------------

[core/om_filestorage_filetransferprotocol.class.php]

Ce fichier permet de déclarer la classe spécialisée pour stocker les fichiers
sur un FTP (ce fichier n'existe pas il est là à titre d'illustration de ce que
le système est capable de faire).



Description du connecteur **alfresco**
--------------------------------------
 
[core/om_filestorage_alfresco.class.php]

Ce fichier permet de déclarer la classe spécialisée pour stocker les fichiers
sur Alfresco (ce fichier n'existe pas il est là à titre d'illustration de ce
que le système est capable de faire).



===========
Utilisation
===========


Les méthodes de la classe d'abstraction sont désormais utilisées dans la classe
upload et dans les widgets upload file du formulaire.

Il est possible de paramétrer une liste de métadonnées d'un champ upload,
certains champs de ce formulaire pouvant contenir certaines informations à
ajouter aux informations du fichier uploadé, il est necessaire de créer le
fichier lors de la validation du formulaire.
Pour ce faire le fichier uploadé sera enregistré temporairement sur le filestorage
défini pour les fichiers temporaires puis enregistré sur le filestorage définitif
lors de la validation du formulaire.

Hors formulaire la méthode create peut être utlisée de 3 façons : 

 - en lui passant le chemin du fichier dans $data et avec le mode défini à "from_path"
 - en lui passant le contenu du fichier dans $data (fonctionnement existant avant
   modification)
 - en lui passant l'UUID d'un fichier temporaire avec le mode défini à "from_temporary"

Configuration du widget Upload
------------------------------

Contraintes
...........

Les contraintes sont à rajouter dans la classe métier de l'objet concerné, dans la méthode setSelect. 

Exemple de configuration de l'ajout de contraintes de contrôles de la taille maximale et de l'extension lors de l'upload de fichier : 

.. code-block :: php

   <?php
       $params = array(
           "constraint" => array(
               "size_max" => 2,
               "extension" => ".pdf;.txt;.odt"
           ),
       );
   ?>

La taille maximale est en mo et la liste des extensions est une chaîne de caractères. 

Métadonnées
...........

Il y a des métadonnées globales et spécifiques.

Les globales sont définies dans [obj/om_db_form.class.php] dans l'attribut $metadata_global.

Exemple de configuration :

.. code-block :: php

   <?php
        var $metadata_global = array(
            "metadonne1" => "méthodeQuiRetourneLaBonneValeur1",
            "metadonne2" => "méthodeQuiRetourneLaBonneValeur2",
        );
    ?>

Les specifiques sont à ajouter en attribut de la classe métier de l'objet concerné. 

Exemple de configuration de l'ajout de métadonnées : 

.. code-block :: php

   <?php
        var $metadata = array(
            "champ" => array(
                "metadonne1" => "méthodeQuiRetourneLaBonneValeur1",
                "metadonne2" => "méthodeQuiRetourneLaBonneValeur2",
                ),
            ),
        );
   ?>

Les clés de ces tableaux sont les noms des métadonnées, les valeurs associées 
sont les noms des méthodes qui retournent les métadonnées.



Récupération du fichier
-----------------------

.. code-block :: php

   //
   $file = $f->storage->get($fic);





Scripts permettant de visualiser / d'accéder au fichier
-------------------------------------------------------

spg/file.php
............

Le script permet de télécharger le fichier.

Le code pour composer le lien vers ce script est le suivant :

.. code-block :: php

   <?php

      $file_download_link = "../spg/file.php?";
      if ($obj != "" && $champ != "" && $id != "") {
          $file_download_link .= "obj=".$obj."&amp;champ=".$champ."&amp;id=".$id;
      } else {
          $file_download_link .= "uid=".$fic;
      }

     
   ?>


spg/voir.php
............

Le script permet de visualiser le fichier.

.. code-block :: php

   <?php

      $file_voir_link = "../spg/voir.php?";
      if ($obj != "" && $champ != "" && $id != "") {
          $file_voir_link .= "obj=".$obj."&amp;champ=".$champ."&amp;id=".$id;
      } else {
          $file_voir_link .= "uid=".$fic;
      }

     
   ?>
