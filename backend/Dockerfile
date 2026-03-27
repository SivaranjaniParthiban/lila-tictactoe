# Stage 1: Build the TypeScript module
FROM node:18-alpine AS builder

WORKDIR /app

# Copy package files if they exist, otherwise init
COPY backend/package*.json ./

# Install esbuild for bundling
RUN npm install --save-dev esbuild || npm init -y && npm install --save-dev esbuild

# Copy source
COPY backend/src ./src

# Bundle main.ts -> build/main.js (Nakama expects a single JS file)
RUN npx esbuild src/main.ts \
    --bundle \
    --platform=node \
    --target=es2017 \
    --outfile=build/main.js \
    --external:@heroiclabs/nakama-runtime

# Stage 2: Final Nakama image
FROM registry.heroiclabs.com/heroiclabs/nakama:3.22.0

COPY --from=builder /app/build/main.js /nakama/data/modules/main.js
COPY backend/config.yml /nakama/data/config.yml

CMD ["/nakama/nakama", "--config", "/nakama/data/config.yml"]
