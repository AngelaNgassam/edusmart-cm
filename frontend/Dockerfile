# ─────────────────────────────────────────────
# Stage 1 : build
# ─────────────────────────────────────────────
FROM node:20-alpine AS builder

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .

# ARG injecté au build par le CI/CD
ARG VITE_API_URL=http://localhost:8000/api
ENV VITE_API_URL=$VITE_API_URL

RUN npm run build

# ─────────────────────────────────────────────
# Stage 2 : serve avec Nginx
# ─────────────────────────────────────────────
FROM nginx:1.27-alpine

# Supprimer la config par défaut
RUN rm /etc/nginx/conf.d/default.conf

# Config Nginx pour SPA React (react-router)
COPY docker/nginx.conf /etc/nginx/conf.d/default.conf

# Copier le build Vite
COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
