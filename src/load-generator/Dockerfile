FROM node:18-slim as builder

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Production stage
FROM node:18-slim

# Create non-root user
RUN groupadd -r appuser && useradd -r -g appuser -u 1001 appuser

WORKDIR /app

# Copy dependencies from builder stage
COPY --from=builder /app/node_modules ./node_modules

# Copy application files
COPY helpers.js scenario.yml ./
COPY package*.json ./

# Change ownership to non-root user
RUN chown -R appuser:appuser /app

# Switch to non-root user
USER 1001

# Expose port (though this is a load generator, not a server)
EXPOSE 3000

# Default command
CMD ["npm", "run", "execute"]