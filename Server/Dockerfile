# Image de base : Node.js version LTS, version légère (alpine)
FROM node:lts-alpine

# Répertoire de travail dans le conteneur
WORKDIR /app

# Copier package.json et package-lock.json
COPY package*.json ./

# Installer les dépendances
RUN npm install

# Copier le reste des fichiers du serveur
COPY . .

# Le serveur écoute sur le port 9000
EXPOSE 9000

# Démarrer l'application
CMD ["npm", "start"]