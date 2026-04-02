FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:0-20

# Install core dependencies + useful dev tools
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    git \
    build-essential \
    ca-certificates \
    openssh-client \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /workspace

# Ensure correct ownership for node user
RUN chown -R node:node /workspace

# Switch to node user
USER node

# Copy only package files first for better layer caching
COPY --chown=node:node package*.json ./

# Install dependencies (use ci if lockfile exists)
RUN if [ -f package-lock.json ]; then npm ci; else npm install; fi

# Copy rest of the source
COPY --chown=node:node . .

# Expose dev port
EXPOSE 3000

# Default command
CMD ["npm", "run", "dev"]