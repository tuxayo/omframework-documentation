.. _methode:

##########
la methode
##########

pour la création d' objets métiers:

- Le développement consiste à créer des objets métier (/obj) qui surchargent la classe abstraite  dbformdyn.class.php
- et modifier les valeurs par défaut




=================================
Surcharger les classes openMairie
=================================


Il vaut mieux utiliser le générateur pour initialiser les classes metiers
Le generateur surcharge la classe dbformdyn.class.php par rapport aux informations de la base ::

    classe abstraite openMairie
                <- classe metier generee
                   depuis la base de données
                            <-  classe metier 1
                                    <- classe metier 2 ...

    dbformdyn.class.php
                <- gen/obj/table.class.php
                                    <- obj/table.class.php

    ou

    dbformdyn.class .php
                <- gen/obj/table.class.php
                                <- obj/table.class.php
                                                <- obj/metier.class.php


Exemple avec openCimetiere ::

    dbformdyn.class.php
                <- gen/obj/emplacement.class.php
                                <-/obj/emplacement.class.php
                                                <- /obj/concession.class.php

=================
valeur par defaut
=================

dans php/dbformdyn.class.php 
(composant openMairie)

Initialisation des valeurs de champ de parametrage formulaire 

Les valeurs suivantes sont mises par defaut afin de pouvoir construire rapidemment un formulaire ::

    valeur par defaut  
       en modification et suppression = initialiser par la valeur des champs dans le SGBD
       en ajout = initialisation vide
   
    type par defaut
       text pour ajout et modification
       hiddenstatic pour suppression
   
    libelle par défaut :
       Lib= nom du champ dans le SGBD
   
    taille et max d un champ
       Taille et max = longueur du champ dans le SGBD
   
    les regroupements et groupements de champs sont vides
   
    les fonctions javascript ne sont pas utilisées

 
===================================
Modification des valeurs par defaut
===================================

dans la classe obj/table.class.php

Voir aussi le generateur 

Les valeurs par defaut sont modifiés par lamethode setVal(nomduchamp, nouvelle valeur)

Les types par defaut sont modifiés par lamethode setType(nomduchamp, nouveau type)

Les longueurs d affichage par defaut sont modifiés par lamethode setTaille(nomduchamp, nouvelle valeur)

Les maximums autorises par defaut sont modifiés par lamethode setMax(nomduchamp, nouvelle valeur)

Les libelles de champ par defaut sont modifiés par la methode setLib(nomduchamp, nouvelle valeur)

Les scripts javascript sont appellés dans la methode setOnchange()



===================
dbformdyn.class.php
===================

composant openMairie

  dbform.class.php


La classe abstraite dbform gère l’interface entre l'objet métier et la base de données connectée via DBPEAR.

Les méthodes principales sont les suivantes :

* orientées sgbd ::

    constructeur
    ajouter : Ajoute un objet
    Modifier : Modifie un objet
    Supprimer : Supprime un objet
    Verifier : Contrôle un objet
    Clesecondaire : Contrôle les cles secondaires
    triggers avant/apres ajout/modification/suppression

* orientees Formulaire ::

    Formulaire : Constitue le formulaire -> appel à formulaire.dyn.class.php
    sousFormulaire : Constitue le sousformulaire -> appel à formulaire.dyn.class.php
    Message : Retourne le message d erreur (contrôle php)
    bouton : Affiche le bouton
    Retour : gére le retour à une interface php en fin de saisie
    sousformulaireRetour : gére le retour à une interface php en fin de saisie de sous formulaire
    setType : Envoi au formulaire les type de champ
    setVal : Envoi au formulaire les valeurs par défaut
    setValSousformulaire : Envoi au sousformulaire les valeurs par défaut
    setlib : Envoi au formulaire les libellés de champs
    setTaille : Envoi au formulaire la taille du champ
    setMax : Envoi au formulaire la taille maximum autorisée du champ
    setSelect : Envoi au formulaire les champs select à afficher
    setOnchange : Envoi au formulaire les controles javascript à effectuer en cas de changement de données dans le champ
    setGroupe : Envoi au formulaire le regroupement de champ par ligne
    setRegroupe : Envoi au formulaire un fieldset

* des fonctions de traitement de champ heure et date::

    DateDB : transforme les dates affichées en date pour base de données
    HeureDB : controle du champs heure saisi 00 ou 00:00 ou 00:00:00
    DateSystemeDB : mise au format base de donnees de la date systeme
    DatePHP : controle et transforme la date saisie (jj/mm/aaaa) en date format PHP

*  des fonctions pour faire des calculs ::

    AnneePHP : controle et recupere l’année de la date saisie (jj/mm/aaaa)
    MoisPHP : controle et recupere le mois de la date saisie (jj/mm/aaaa)
    JourPHP : controle et recupere le jour de la date saisie (jj/mm/aaaa)

La classe dbform.class.php fait appel à la classe formulaire.dyn.class.php pour afficher le formulaire.
