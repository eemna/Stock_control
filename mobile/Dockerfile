# --- Étape 1 : Build de l'application Expo Web ---
FROM node:22-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

# Variable d’environnement passée depuis docker-compose
ARG API_URL
ENV API_URL=${API_URL}

# ✅ Compiler en incluant la variable d’environnement dans le build
RUN echo "Building Expo Web with API_URL=${API_URL}"

# Compiler le projet
RUN npx expo export --platform web


# --- Étape 2 : Image finale ultra-légère ---
FROM nginx:1-alpine

# Supprimer la page par défaut de Nginx
RUN rm -rf /usr/share/nginx/html/*

# Copier le build généré par Expo
COPY --from=builder /app/dist /usr/share/nginx/html

# Exposer le port web standard
EXPOSE 80

# Lancer Nginx
CMD ["nginx", "-g", "daemon off;"]
