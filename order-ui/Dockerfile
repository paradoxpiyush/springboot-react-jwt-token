# Step 1: Build stage
FROM node:18 AS builder

# Set working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json (if present)
COPY package.json package-lock.json ./

# Install dependencies
RUN npm ci

# Copy the entire project
COPY . .

# Build the React app
RUN npm run build

# Step 2: Serve stage
FROM nginx:alpine

# Copy the built files to Nginx's HTML directory
COPY --from=builder /app/build /usr/share/nginx/html
COPY --from=builder /app/nginx.conf /etc/nginx/conf.d/default.conf

# Expose Nginx default port
EXPOSE 80

# Health check
HEALTHCHECK --interval=30s --timeout=3s CMD wget -qO- http://localhost:80 || exit 1

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
