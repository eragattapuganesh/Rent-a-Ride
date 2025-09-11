# client/Dockerfile
FROM node:18-alpine AS build
WORKDIR /app
COPY client/package*.json ./
RUN npm ci
COPY client/ ./
# build (Vite creates /dist typically)
RUN npm run build

FROM nginx:stable-alpine
# SPA: serve built files
COPY --from=build /app/dist /usr/share/nginx/html
# SPA fallback for client-side routing
COPY docker/nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
