# TP4 : Docker et Docker Compose (application MERN)

## Objectif
Containeriser une application MERN (MongoDB, Express, React, Node.js) avec Docker, l'orchestrer avec Docker Compose, puis la déployer avec un pipeline Jenkins.

## Liens
- GitHub : https://github.com/irada-collab/MyAppReact
- Docker Hub (serveur) : https://hub.docker.com/r/iradadev2026/mern-server
- Docker Hub (client) : https://hub.docker.com/r/iradadev2026/mern-client

## Étapes réalisées (avec explications)

### 1. Clonage du projet
Commande : `git clone https://github.com/wiem-kb/MyAppReact.git`
Je télécharge le projet de la prof sur mon ordinateur. Il contient deux dossiers : `client` (la partie visible, en React) et `server` (la partie qui travaille en arrière-plan, en Express).

### 2. Dockerfile du serveur
Un Dockerfile est une recette qui explique à Docker comment préparer mon application.
- `FROM node:lts-alpine` : je pars d'une petite image qui contient déjà Node.js.
- Je copie les fichiers `package*.json`, puis `npm install` installe les outils dont le serveur a besoin.
- Je copie le reste du code, j'indique le port 9000 (`EXPOSE 9000`), et `npm start` démarre le serveur.

### 3. Dockerfile du client
C'est la même idée que pour le serveur, avec deux différences :
- `npm run build` transforme le code React en fichiers prêts à être affichés (dossier `dist`).
- J'installe `serve`, un petit programme qui envoie ces fichiers au navigateur sur le port 3000.

### 4. Construction des images
Commandes : `docker build -t mern-server ./server` et `docker build -t mern-client ./client`
Docker lit chaque Dockerfile et fabrique une image. Une image est comme un modèle de l'application, prêt à être lancé. L'option `-t` donne un nom à l'image.

### 5. Création du réseau
Commande : `docker network create mern-network`
Je crée un réseau privé pour que les conteneurs puissent se parler entre eux (le serveur doit parler à MongoDB, par exemple).

### 6. Lancement avec `docker run`
Je lance les trois conteneurs à la main : MongoDB (la base de données), le serveur et le client. Un conteneur est une image qui tourne. Je les mets tous dans le réseau `mern-network`. L'option `-p` permet d'ouvrir l'application dans mon navigateur (par exemple `localhost:3000`).

### 7. Fichier docker-compose.yml
Lancer trois conteneurs à la main, c'est long. Avec ce fichier, je décris tout au même endroit :
- trois services : `mongodb`, `server` et `client` ;
- le serveur démarre après MongoDB, et le client après le serveur (`depends_on`) ;
- la variable `MONGO_URI` indique au serveur où trouver la base de données ;
- le réseau `mern-network` est de type `bridge`.

### 8. Lancement avec Docker Compose
Commande : `docker compose up --build`
Une seule commande construit les images et démarre les trois conteneurs. L'application est visible sur http://localhost:3000.

### 9. Jenkinsfile
Jenkins est un outil qui fait ces étapes automatiquement. Le Jenkinsfile décrit quatre stages (étapes) :
1. **Clonage** : Jenkins télécharge le projet depuis GitHub.
2. **Nettoyage** : il arrête les anciens conteneurs.
3. **Build** : il construit les images.
4. **Lancement** : il démarre l'application.
Quand les quatre stages sont verts, l'application est bien déployée.

### 10. Envoi des images sur Docker Hub
Docker Hub est un site où l'on partage des images.
- `docker tag` donne à mon image un nom qui contient mon compte (par exemple `iradadev2026/mern-server`).
- `docker push` envoie l'image sur le site.
Ainsi, tout le monde peut la télécharger.

## Lancer l'application
    docker compose up --build

Client : http://localhost:3000 / Serveur : http://localhost:9000

## Problèmes rencontrés
- Erreur 404 sur le client : le dossier du build de React est `dist` (Vite) et non `build`. Le Dockerfile a été corrigé.
- Jenkins ne trouvait pas la commande `docker` : le chemin complet de `docker.exe` et le dossier Docker dans le PATH ont été indiqués dans le Jenkinsfile.

## Captures
Voir les captures jointes : pipeline Jenkins, images, conteneurs, Docker Hub.