# backend/Dockerfile
FROM node:20-alpine
WORKDIR /app

# Install only production deps
COPY backend/package*.json ./
RUN npm ci --omit=dev

# Copy source
COPY backend/ ./

ENV NODE_ENV=production
# The app should respect PORT env
ENV PORT=8080
EXPOSE 8080

# Prefer "npm start"; if your package.json lacks it, change to your entry file (e.g., server.js)
CMD ["npm","start"]
