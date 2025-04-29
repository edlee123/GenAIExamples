# Copyright (C) 2024 Intel Corporation
# SPDX-License-Identifier: Apache-2.0

# Use node 20.11.1 as the base image
FROM node:20.11.1 as base
FROM base as builder


# Update package manager and install Git
RUN apt-get update -y && apt-get install -y git

# Copy the front-end code repository
WORKDIR /app
COPY ./svelte/package*.json ./
RUN npm ci
COPY ./svelte/ ./
RUN npm run build
RUN npm prune --production

FROM base
WORKDIR /app
COPY --from=builder /app/build build/
COPY --from=builder /app/node_modules node_modules/
COPY --from=builder /app/package.json .
EXPOSE 3000
ENV NODE_ENV=production
# Fixing a UID. ALlows testing locally in docker
USER 1001
CMD [ "node", "build" ]
