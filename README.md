# TP4 : Docker et Docker Compose (application MERN)

## Objectif
Containeriser une application MERN (MongoDB, Express, React, Node.js) avec Docker, l'orchestrer avec Docker Compose, puis la déployer avec un pipeline Jenkins.

## Liens
- GitHub : https://github.com/irada-collab/MyAppReact
- Docker Hub (serveur) : https://hub.docker.com/r/iradadev2026/mern-server
- Docker Hub (client) : https://hub.docker.com/r/iradadev2026/mern-client

## Étapes réalisées
1. Clonage du projet : `git clone https://github.com/wiem-kb/MyAppReact.git`
2. Dockerfile du serveur (image de base `node:lts-alpine`, port 9000).
3. Dockerfile du client (build React, serveur léger `serve`, port 3000).
4. Construction des images : `docker build -t mern-server ./server` et `docker build -t mern-client ./client`
5. Création du réseau : `docker network create mern-network`
6. Lancement de MongoDB, du serveur et du client avec `docker run`.
7. Fichier `docker-compose.yml` (services mongodb, server, client, réseau mern-network en bridge).
8. Lancement de l'application : `docker compose up --build`
9. Jenkinsfile avec 4 stages : Clonage, Nettoyage, Build, Lancement.
10. Envoi des images sur Docker Hub avec `docker tag` et `docker push`.

## Lancer l'application
    docker compose up --build

Client : http://localhost:3000 / Serveur : http://localhost:9000

## Problèmes rencontrés
- Erreur 404 sur le client : le dossier du build de React est `dist` (Vite) et non `build`. Le Dockerfile a été corrigé.
- Jenkins ne trouvait pas la commande `docker` : le chemin complet de `docker.exe` et le dossier Docker dans le PATH ont été indiqués dans le Jenkinsfile.

## Captures
Voir les captures jointes : pipeline Jenkins, images, conteneurs, Docker Hub.