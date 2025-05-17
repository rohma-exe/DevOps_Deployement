# Stage 1: Build
FROM node:20 as build
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build

# Stage 2: Serve
FROM node:20-alpine
WORKDIR /app
RUN npm install -g serve
COPY --from=build /app/dist /app/dist
EXPOSE 3000
CMD ["serve", "-s", "dist", "-l", "3000"]
