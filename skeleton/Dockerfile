# Build stage
FROM node:16-alpine as build

# Set working directory
WORKDIR /app

# Add arguments
ARG DEPLOYMENT_STAGE=development
ARG BUILD_COMMAND=build:development

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy all files
COPY . .

# Build using the appropriate command based on deployment stage
RUN npm run build:${DEPLOYMENT_STAGE}

# Serve stage
FROM nginx:alpine

# Copy built assets from build stage
COPY --from=build /app/build /usr/share/nginx/html

# Add your nginx.conf
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 8080
EXPOSE 8080

# Start Nginx and keep it running
CMD ["nginx", "-g", "daemon off;"]