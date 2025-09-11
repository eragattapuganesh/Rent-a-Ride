# backend/Dockerfile
FROM node:18-alpine AS build

WORKDIR /app
# copy package files first for layer caching
COPY backend/package*.json ./
RUN npm ci --production

# copy app source
COPY backend/ ./

ENV NODE_ENV=production
EXPOSE 5000

# Use start script from package.json (ensure backend/package.json has "start")
CMD ["npm", "start"]

