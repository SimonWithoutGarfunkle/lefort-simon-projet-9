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
