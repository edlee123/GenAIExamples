# Copyright (C) 2024 Intel Corporation
# SPDX-License-Identifier: Apache-2.0

# Use node 20.11.1 as the base image
FROM node:20.11.1 AS base
FROM base AS builder

# Update package manager and install Git
RUN apt-get update -y && apt-get install -y git

# Set up build environment
WORKDIR /app
COPY ./svelte/package*.json ./
RUN npm ci
COPY ./svelte/ ./
RUN npm run build
RUN npm prune --production

# Final production image
FROM base
WORKDIR /app
COPY --from=builder /app/build build/
COPY --from=builder /app/node_modules node_modules/
COPY --from=builder /app/package.json .

ENV NODE_ENV=production
ENV PORT=5173
EXPOSE ${PORT}

USER 1001

# Run the production build with port configuration
CMD ["sh", "-c", "node build/index.js --port ${PORT} --host 0.0.0.0"]
