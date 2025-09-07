# Stage 1: Install dependencies
FROM node:18-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install

# Stage 2: Build the React application
FROM node:18-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

# Stage 3: Serve the app with Nginx
FROM nginx:1.25-alpine
# Copy the built assets from the builder stage
COPY --from=builder /app/build /usr/share/nginx/html
# Copy our custom Nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]