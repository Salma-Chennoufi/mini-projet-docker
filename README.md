#  Mini-Projet Docker : Student List

##  Objectif
Ce projet consiste à dockeriser une application Python Flask (backend) et PHP (frontend) avec Docker.

## Installation et Exécution
###  Cloner le repository

git clone https://github.com/Salma-Chennoufi/mini-projet-docker.git

## I. Construire (build) et tester l'API

###  Dockerfile 

![](/screenshots/image-1.png)

**Utiliser l'image officielle de Python 3.8 basée sur Debian Buster**. Cela garantit que nous avons un environnement Python fonctionnel avec les outils nécessaires.


![](/screenshots/image-2.png)

 **Ajouter des métadonnées sur l’auteur de l’image Docker**. Cela permet de savoir qui a construit cette image.


![](/screenshots/image-3.png)

**Copier le fichier `requirements.txt` dans le conteneur**. Cela permet d’installer les dépendances Python.


![](/screenshots/image-4.png)

**Mettre à jour le système et installe des bibliothèques nécessaires** pour que certaines dépendances Python puissent être compilées correctement.


![](/screenshots/image-5.png)

**Installer les dépendances Python définies dans `requirements.txt`**.


![](/screenshots/image-6.png)

**Déclarer `/data` comme un volume Docker** pour stocker les données de manière persistante, comme le fichier `student_age.json`.


![](/screenshots/image-7.png)

**Copier le fichier `student_age.py` (le code source de l’API) et `student_age.json` (les données des étudiants) dans le conteneur**.


![](/screenshots/image-8.png)

**Indiquer que le conteneur utilisera le port 5000**, afin que l’API soit accessible depuis l’extérieur.


![](/screenshots/image-9.png)

**Définir la commande par défaut à exécuter au démarrage du conteneur**. 



### Test:

![](/screenshots/image-10.png)

**Créer une image Docker** nommée student_list en utilisant le Dockerfile situé dans le répertoire courant (.).

![](/screenshots/image-11.png)


![](/screenshots/image-12.png)

 **Créer et exécuter un conteneur Docker** nommé student_list_c, lier le port 5000 de l'hôte au port 5000 du conteneur pour accéder à l'API, et monter le fichier student_age.json de l'hôte dans /data/student_age.json du conteneur pour conserver les données, en utilisant l'image student_list.


![](/screenshots/image-13.png)

![](/screenshots/image-14.png)

**Envoyer une requête GET** à l'API en utilisant l'authentification root:root pour récupérer la liste des âges des étudiants.
**L'API répond correctement**


## II. Infrastructure as Code (docker-compose.yml)

### Service API:

![](/screenshots/image15.png)

Le service **API** utilise l’image **student_list** pour exécuter l’API Flask, monte le fichier **student_age.json** dans /data pour la persistance des données, expose le port 5000 pour l’accès externe et est connecté au réseau **student_list_network** pour communiquer avec les autres services.


### Service website:

![](/screenshots/image16.png)

Le service **website** utilise l’image **php:apache** pour exécuter l’interface utilisateur, monte les fichiers du site web dans /var/www/html, définit des variables d’environnement pour l’authentification, expose le port 8080 pour l’accès au site et dépend du service api, garantissant que l’API démarre avant lui.


### Networks:

![](/screenshots/image17.png)

Définit un réseau Docker personnalisé pour la communication entre API et Website


### Lancement de docker-compose.yml

![](/screenshots/image18.png)
![](/screenshots/image19.png)


**La dockerisation de l'application SUPMIT a été réalisée avec succès!**
![](/screenshots/image20.png)


## III. Docker Registry

### Service registry:

![](/screenshots/image25.png)

Ce service déploie un registre Docker privé accessible sur le port 5000 et connecté au réseau registry_network.

### Service registry-ui:

![](/screenshots/image26.png)

Ce service déploie une interface web pour gérer le registre Docker privé, accessible sur le port 8081 et dépendant du service registry.

### Lancement de docker-compose-registry.yml

![](/screenshots/image21.png)


![](/screenshots/image22.png)


![](/screenshots/image23.png)
Ces commandes taguent l’image student_list pour l’associer au registre privé localhost:5000, puis la poussent vers ce registre pour qu’elle puisse être stockée et réutilisée.


![](/screenshots/image24.png)
L'image **student_list** existe dans le registre privé.


# Déploiement Automatisé d'une Application Web avec CI/CD sur Jenkins et AWS : Static App

## Objectif
Déployer une application web statique dans différents environnements de déploiement (simulés sur AWS).

## Pipeline CI/CD sur Jenkins

### CI (Continuous Integration):

   **1. Build:** Dockerfile
![](/static_app/screenshots/image.png)

Construction de l'image Docker:
![](/static_app/screenshots/image-2.png)
![](/static_app/screenshots/image-1.png)

   **2. Test:**

Exécuter l’image Docker en local:
![](/static_app/screenshots/image-3.png)

Tester l’accessibilité avec curl:
![](/static_app/screenshots/image-4.png)

   **3. Release:** 

Pousser l’image sur Docker Hub.
![](/static_app/screenshots/image-5.png)
![](/static_app/screenshots/image-6.png)
![](/static_app/screenshots/image-7.png)


### CD (Continuous Deployment):

**jenkinsfile**

La section 'environment' définit une variable d'environnement 'DOCKER_IMAGE', qui spécifie le nom et la version de l'image Docker à construire, ici 'salma171/static_app:latest'. Cette variable sera utilisée dans les étapes suivantes du pipeline pour construire et déployer l'image Docker.

![](/static_app/screenshots/image27.png)

La section 'stages' définit les différentes étapes du pipeline. 
La première étape, nommée 'Checkout', permet de récupérer le code source du dépôt Git. 
La commande 'checkout scm' indique à Jenkins de cloner automatiquement le dépôt associé au projet, tandis que 'echo' affiche un message dans la console pour indiquer le début de cette opération.

![](/static_app/screenshots/image28.png)

L'étape 'Build' a pour objectif de construire une image Docker à partir du Dockerfile présent dans le projet. 
Le message 'Building the Docker image...' est affiché pour indiquer le début du processus. 
Ensuite, la commande 'docker build -t $DOCKER_IMAGE .' est exécutée via un bloc 'script' pour construire l'image en utilisant la variable définie précédemment ($DOCKER_IMAGE).

![](/static_app/screenshots/image29.png)

L'étape 'Deploy' est chargée de déployer l'application. 
Le message 'Deploying the application...' informe que le déploiement est en cours. 
La commande 'docker run -d -p 80:80 $DOCKER_IMAGE' lance un conteneur en arrière-plan à partir de l'image Docker construite précédemment, en exposant le port 80 du conteneur sur le port 80 de la machine hôte, rendant ainsi l'application accessible via le navigateur.

![](/static_app/screenshots/image30.png)

La section 'post' contient des actions à exécuter une fois le pipeline terminé, quel que soit le résultat. 
Le bloc 'always' s'exécute dans tous les cas : ici, il affiche un message et nettoie les ressources Docker inutilisées avec 'docker system prune -f'.
Le bloc 'success' s'exécute uniquement si le pipeline se termine avec succès, et affiche un message de confirmation.
Le bloc 'failure' s'exécute si le pipeline échoue, et affiche un message d'erreur.

![](/static_app/screenshots/image31.png)


