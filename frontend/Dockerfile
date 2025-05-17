# frontend/Dockerfile

# Step 1: Build app
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Step 2: Serve built app
FROM node:20-alpine

WORKDIR /app
RUN npm install -g serve

# Copy from builder
COPY --from=builder /app/dist ./dist

# Expose a different port than Part-I
EXPOSE 3001

CMD ["serve", "-s", "dist"]
