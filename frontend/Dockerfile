# Étape 1 : construire l'application Angular
FROM node:22 AS build

WORKDIR /app

# Copier les fichiers de dépendances
COPY package*.json ./

# Installer les dépendances
RUN npm install

# Copier le reste du projet
COPY . .

# Construire l'application Angular
RUN npm run build


# Étape 2 : servir l'application avec Nginx
FROM nginx:alpine

# Copier les fichiers générés par Angular
COPY --from=build /app/dist/*/browser /usr/share/nginx/html

# Exposer le port HTTP
EXPOSE 80

# Lancer Nginx
CMD ["nginx", "-g", "daemon off;"]
