# lefort-simon-projet-9
# Projet MédiLabo Solutions pour la formation OpenClassRooms
Médilabo - We care for you
Notre application travaille avec des cliniques de santé et des cabinets privés sur le dépistage des risques de maladies notamment le diabète.
Il s'agit d'une application web pour ordinateur.

Ceci est le projet n°9 du parcours de formation Développeur Java par OpenClassRooms. L'objectif est de concevoir une application Web de A à Z en microservice.

## Guide d'installation

Ce programme utilise principalement les téchnologies suivantes :
- Java 17
- Spring Boot 3.1.5
- Spring Security 6.1.5
- Spring Cloud Gateway 4.0.4
- Maven 3.9.1
- Mysql 8.0.17
- MongoDB 4.1.5
- Thymeleaf 3.1.5

### Prerequis

Pour installer et lancer ce programme, vous devez disposez de la configuration suivante :
- Docker Engine

### Installation

1.Télécharger le projet Git

Télécharger et dézipper ce projet.
Telecharger et dézipper chacun des microservices dans le projet principal


2.Construire les images

Depuis le dossier racine, utiliser la commande : "docker-compose build" 
Selon la vitesse de la connexion internet, cette étape peut prendre plusieurs minutes.

3.Ports

Par défaut, le programme utilise les ports suivants. Assurez vous qu'ils soient bien disponibles. Si besoin, vous pouvez modifier ces paramètres.
- Microservice front : 8080
- Microservice gateway : 9000
- Microservice patients : 9001
- Microservice doctor : 9002
- Microservice assessment : 9003
- Base MySQL : 3307
- Base MongoDB : 27018



### Lancer le programme

Les images sont désormais disponibles sur Docker. Vous pouvez démarrer le projet depuis le dossier principal "lefort-simon-projet-9-main" avec la commande : "docker-compose up"
L'application est désormais accessible ! http://localhost:8080/login

Pour simplifier la découverte du programme, l'écran de login est pré-rempli avec les identifiants d'un compte.


### Présentation des microservices

**Patients :**
Gestion de la liste des patients et leur données personnelles.

**Doctor :**
Gestion des notes des medecins. Ces notes constituent l'historique médical de chaque patient.

**Assessment :**
Service d'évaluation du risque de diabète. Ce service requete les services Patients et Doctor pour récupérer toutes les infos sur un patient (données personnelles + historique médical). Il analyse ensuite toutes ses données pour évaluer le risque de diabètes. Il s'appuie notamment sur l'age et le genre du patient et il recherche des mots clés dans son historique.

**Front :**
Le front est fait en HTML et CSS avec Thymeleaf. Il dispose d'un microservice dédié.
Le front est entièrement sécurisé. Il faut etre identifié pour l'utiliser mais pour simplifier la démo, l'écran de login est pré-rempli avec les identifiants d'un compte.
Le front envoie ses requetes à la Gateway.

**Gateway :**
Les 3 services Back (Patients, Doctor, Assessment) sont situées dernière la Gateway. Les requetes sont envoyées à la Gateway qui les redirigent vers les microservices adéquates.
La Gateway front est entièrement sécurisé. Il faut etre identifié pour l'utiliser (memes identifiants que le front).



### Ressources
Le dépot GitHub contient tous les éléments.

**Bases de données :**
Le microservice Patients utilise la base de données MySQL "patientsdb". Elle est pré remplie avec 4 patients pour la démo.
Le microservice Doctor utilise la base de données MongoDB "doctorsdb". Elle est pré remplie avec les notes de médecin pour les 4 patients de la base patientsdb.

# Green Code
Le numérique est une source de pollution mondiale. Ce secteur représente 2,5% de l’empreinte carbone en France (rapport Arcep 2022).
Les principales problématiques mises en avant sont :
 - La consommation excessive de l’espace ou des ressources du disque dur par les logiciels.
 - Les logiciels contiennent de nombreuses fonctionnalités inutiles et non essentielles.
Il est donc essentiel d'avoir ces éléments en tete dans toutes les étapes de la création d'un projet.

Le Front-End joue un role essentiel dans cette démarche. En tant qu'interface avec l'utilisateur, le Front permet de savoir précisement comment l'outil est utilisé et donc d'identifier les fonctionnalités clés et les fonctionnalités inutilisées qui pourront etre retirées.
Concretement, un Front responsable, c'est un front léger (sans animations et images inutiles) et avec des requetes optimisées.

Pour le Back-End, la consommation de ressources dépend de la [compléxité algorythmique](https://fr.wikipedia.org/wiki/Analyse_de_la_complexit%C3%A9_des_algorithmes#Complexit%C3%A9,_comparatif). Il est important de sensibiliser les développeurs à cette notion. Une compléxité algorythmique non maitrisée peut transformer un programme leger et rapide lors d'une démo en un gouffre à mémoire, extremement lent en production. La mauvaise pratique la plus courante est la compléxité O(n²) qui correspond au parcours d'un tableau a 2 dimensions (une imbrication de boucle "for" par exemple) :

Enfin pour la base de données, la redondance des données est la principale cause de surconsommation (il s'agit ici de la redondance non maitrisée et non de la replication à des fins de sauvegarde ou de maintien d'up-time). Une bonne pratique pour éviter la redondance est de [normaliser la base de données](https://fr.wikipedia.org/wiki/Forme_normale_(bases_de_donn%C3%A9es_relationnelles)).
En appliquant systématiquement au moins la 2e forme normale, on limite grandement le risque de doublons inutils, et donc de stockage inutile.

## Application du Green Code à Médilabo
Les bons points :
 - Les bases de données sont normées, les données ne sont pas redondantes. Par exemple, la base MongoDB se limite à l'ID patient, elle ne contient pas son nom et son prénom.
 - La compléxité algorithmique est faible . Il n'y a aucune imbrication de boucle.
 - L'application est bien logé. Grâce a ces logs, il est facile d'identifier les fonctionnalités inutilisées pour alléger l'application.

Les points d'amélioration :
 - l'architecture en microservice présente de nombreux avantages pour une grosse application. Elle sera certainement justifiée quand ce programme atteindra une taille critique mais actuellement ceci semble être de l'[overengineering](https://en.wikipedia.org/wiki/Overengineering). Les notes des docteurs qui constituent l'historique des patients n'ont pas de raison d'exister en dehors du patient. Une unique base MongoDB pour le projet semble indiquée. De même, l'ensemble du projet pourrait etre regroupé en 1 service (voir 2 avec le service Front et le service Back si cela reflète mieux les équipes de travaille). Ainsi, au lieu de disposer de 5 containers (les 3 backs, le front et la gateway) avec Java et Spring Boot, le programme tournerai avec 1 ou 2 seulement. L'espace de stockage requis serait diviser par 3.
 - Mise en place d'outils pour mesurer la consommation de ressources. On pourra ainsi identifier les composants les plus consommateurs pour les refactorer.
 - La base de données patients contient des informations de contact qui ne sont jamais utilisées. Si elles ne sont pas necessaires, elles peuvent etre retirées.
