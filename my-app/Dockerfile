# Stage 1: Build the React app
FROM node:alpine3.18 AS build

WORKDIR /my-app

COPY package*.json ./

RUN npm ci --omit=dev

COPY . .

RUN npm run build

# Stage 2: Serve app with Nginx
FROM nginx:alpine

# Remove default nginx static assets and replace with React build output
RUN rm -rf /usr/share/nginx/html/*

COPY --from=build /my-app/build /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
