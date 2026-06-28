# ---------- BUILD STAGE ----------
FROM node:20-alpine AS builder

WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm install

# Copy source code
COPY . .

# Compile TypeScript
RUN npm run build


# ---------- PRODUCTION STAGE ----------
FROM node:20-alpine

WORKDIR /app

ENV NODE_ENV=development

# Copy package files
COPY package*.json ./

# Install all dependencies (drizzle-kit is needed if you run migrations in the container)
RUN npm install

# Copy compiled application
COPY --from=builder /app/dist ./dist

# Copy Drizzle configuration
COPY --from=builder /app/drizzle.config.ts ./drizzle.config.ts

# Copy schema files used by Drizzle
COPY --from=builder /app/src ./src

EXPOSE 3000

CMD ["node", "dist/index.js"]
