# backend/Dockerfile

FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy only dependency files first
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the code
COPY . .

# Expose port used by your backend (can be different from Part-I)
EXPOSE 8889

# Use entry point (update if it's not server.js)
CMD ["node", "server.js"]
