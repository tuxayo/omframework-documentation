.. _filestorage:

####################################################
Abstraction du 'filestorage' (stockage des fichiers)
####################################################

.. warning::

   Cette rubrique est en cours de rédaction.

****************
Principe général
****************

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




**************
Fonctionnement
**************

Description de l'abstracteur
****************************

[core/om_filestorage.class.php]

Ce fichier est composé de deux classes : la classe d'abstraction, la classe de
base pour les classes spécialisées.

La classe 'filestorage' est une classe d'abstraction de stockage de fichiers.
C'est cette classe qui est instanciée et utilisée par d'autres scripts pour
gérer la création, récupération, suppression de fichiers et ce peu importe le
stockage utilisé. Son objectif est d'instancier la classe de stockage spécifique
aussi appelée plugin de stockage correspondant au paramétrage sélectionné. Cette
classe de stockage spécifique hérite de la classe 'base_storage' qui lui sert de
modèle.


Description du fichier de configuration
***************************************

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

Ce fichier s'inspire des autres fichiers de configuration (mail.inc.php,
directory.inc.php, ...). Il doit contenir un tableau associatif : ::

    $filestorage = array();

Exemple d'un paramétrage (la clé filestorage-default doit être rajoutée dans
une des valeurs du paramétrage du fichier database.inc.php comme l'est
mail-default ou directory-default) : ::

    $filestorage["filestorage-default"] = array {
        "storage" => "filesystem", // l'attribut storage est obligatoire
        "path" => "/var/www/openfoncier/data/",
        ...
    }



Description du connecteur **depredacted**
*****************************************

[core/om_filestorage_deprecated.class.php]

Cette classe est une classe de stockage spécifique aussi appelée plugin de
stockage pour le système d'abstraction de stockage des fichiers. Le principe de
ce plugin est de stocker tous les fichiers à plat selon la méthode utilisée
avant la création du système de stockage. Ce plugin a été créé uniquement dans
un soucis de garder la compatibilité pour les applications existantes.

Description du connecteur **filesystem**
****************************************

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
**************************************************

[core/om_filestorage_filetransferprotocol.class.php]

Ce fichier permet de déclarer la classe spécialisée pour stocker les fichiers
sur un FTP (ce fichier n'existe pas il est là à titre d'illustration de ce que
le système est capable de faire).



Description du connecteur **alfresco**
**************************************
 
[core/om_filestorage_alfresco.class.php]

Ce fichier permet de déclarer la classe spécialisée pour stocker les fichiers
sur Alfresco (ce fichier n'existe pas il est là à titre d'illustration de ce
que le système est capable de faire).



***********
Utilisation
***********



Les méthodes de la classe d'abstraction sont désormais utilisées dans la classe
upload et dans les widgets upload file du formulaire.



