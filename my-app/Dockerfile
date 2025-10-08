# -------------------------------------------------------
# STAGE 1 — Build the React/Vite Application
# -------------------------------------------------------
FROM node:22.12.0-bullseye AS build
# (✅ Node 22.12+ is stable and compatible with latest Vite)

# Set working directory inside container
WORKDIR /my-app

# Copy package files first for better caching
COPY package*.json ./

# Install dependencies (using npm ci when package-lock.json exists)
RUN if [ -f package-lock.json ]; then npm ci --legacy-peer-deps; else npm install --legacy-peer-deps; fi

# Copy all source code into the container
COPY . .

# Build optimized production files
RUN npm run build

# -------------------------------------------------------
# STAGE 2 — Serve the app using Nginx
# -------------------------------------------------------
FROM nginx:1.27.2-alpine

# Remove default Nginx static files
RUN rm -rf /usr/share/nginx/html/*

# Copy the built app from the previous stage
COPY --from=build /my-app/dist /usr/share/nginx/html

# Optional: copy custom nginx.conf if you need SPA routing
# COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose the default Nginx HTTP port
EXPOSE 80

# Run Nginx in foreground mode
CMD ["nginx", "-g", "daemon off;"]
