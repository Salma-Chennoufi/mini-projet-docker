#  Mini-Projet Docker : Student List

##  Objectif
Ce projet consiste à dockeriser une application Python Flask (backend) et PHP (frontend) avec Docker.

##  Structure du projet
mini-projet-docker/
│── student_list
│   │simple_api/
│       ├── student_age.py
│       ├── student_age.json
│       ├── requirements.txt
│       ├── Dockerfile
│
│── website/
│   ├── index.php
│ 
│
│── docker-compose.yml
│── docker-compose-registry.yml
│── README.md


## Installation et Exécution
###  Cloner le repository

git clone https://github.com/Salma-Chennoufi/mini-projet-docker.git

I. Construire (build) et tester l'API

###  Dockerfile 

![](image-1.png)

**Utilise l'image officielle de Python 3.8 basée sur Debian Buster**. Cela garantit que nous avons un environnement Python fonctionnel avec les outils nécessaires.

![](image-2.png)

 **Ajoute des métadonnées sur l’auteur de l’image Docker**. Cela permet de savoir qui a construit cette image.

![](image-3.png)

**Copie le fichier `requirements.txt` dans le conteneur**. Cela permet d’installer les dépendances Python.

![](image-4.png)

**Met à jour le système et installe des bibliothèques nécessaires** pour que certaines dépendances Python puissent être compilées correctement.

![](image-5.png)

**Installe les dépendances Python définies dans `requirements.txt`**.

![](image-6.png)

**Déclare `/data` comme un volume Docker** pour stocker les données de manière persistante, comme le fichier `student_age.json`.

![](image-7.png)

**Copie le fichier `student_age.py` (le code source de l’API) et `student_age.json` (les données des étudiants) dans le conteneur**.

![](image-8.png)

**Indique que le conteneur utilisera le port 5000**, afin que l’API soit accessible depuis l’extérieur.

![](image-9.png)

**Définit la commande par défaut à exécuter au démarrage du conteneur**. 

### Test

![](image-10.png)

**Créer une image Docker** nommée student_list en utilisant le Dockerfile situé dans le répertoire courant (.).

![](image-11.png)


![](image-12.png)

 **Créer et exécuter un conteneur Docker** nommé student_list_c, lier le port 5000 de l'hôte au port 5000 du conteneur pour accéder à l'API, et monter le fichier student_age.json de l'hôte dans /data/student_age.json du conteneur pour conserver les données, en utilisant l'image student_list.


![](image-13.png)

![](image-14.png)

**Envoyer une requête GET** à l'API en utilisant l'authentification root:root pour récupérer la liste des âges des étudiants.
**L'API répond correctement**

