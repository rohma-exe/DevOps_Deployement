# Use Node full image (not Alpine) for compatibility
FROM node:20

WORKDIR /app

# Copy only package files first for caching
COPY package*.json ./

# Install dependencies including module-alias
RUN npm install

# Copy the rest of your backend files
COPY . .

EXPOSE 8888

CMD ["node", "server.js"]
