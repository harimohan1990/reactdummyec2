# ---------------------------------------------
# STAGE 1 — Build the React/Vite application
# ---------------------------------------------
FROM node:20.19.0-alpine AS build

# Set working directory inside the container
WORKDIR /my-app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm ci

# Copy all source code into container
COPY . .

# Build the optimized production version
RUN npm run build

# ---------------------------------------------
# STAGE 2 — Run the app using Nginx
# ---------------------------------------------
FROM nginx:alpine

# Remove default Nginx static assets
RUN rm -rf /usr/share/nginx/html/*

# Copy build output from previous stage
COPY --from=build /my-app/dist /usr/share/nginx/html

# Copy a custom Nginx configuration (optional, if you want routing support)
# Uncomment if you add a custom nginx.conf file
# COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose the default Nginx port
EXPOSE 80

# Run Nginx in foreground mode
CMD ["nginx", "-g", "daemon off;"]
