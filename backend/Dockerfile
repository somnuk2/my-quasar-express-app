# Stage 1: Build dependencies
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./
# Install only production dependencies
RUN npm ci --only=production

# Stage 2: Production
FROM node:20-alpine

WORKDIR /app

# Set ownership of the working directory to the non-root user 'node'
RUN chown -R node:node /app

# Switch to non-root user
USER node

# Copy dependencies from builder
COPY --from=builder --chown=node:node /app/node_modules ./node_modules

# Copy application code
COPY --chown=node:node . .

# Ensure logs directory exists (optional, as code creates it, but good practice)
# Since /app is owned by node, the app can create it. 

# Healthcheck
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
  CMD wget --quiet --tries=1 --spider http://localhost:3000/api/demo || exit 1

EXPOSE 3000

CMD ["node", "server.js"]