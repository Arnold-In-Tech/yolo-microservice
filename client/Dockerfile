# Use an official Node runtime as a parent image
FROM node:14-slim AS build

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy the package.json and package-lock.json files to the container
COPY package*.json ./

# Install application dependencies
RUN npm install

# Copy the rest of the application code to the container
COPY . .

FROM alpine:3.9

WORKDIR /app

RUN apk update && apk add --update nodejs npm && \
    echo 'http://dl-cdn.alpinelinux.org/alpine/v3.9/main' >> /etc/apk/repositories && \		
    echo 'http://dl-cdn.alpinelinux.org/alpine/v3.9/community' >> /etc/apk/repositories		# Alpine 3.9 Repository
    
COPY --from=build /usr/src/app /app

# Expose the port the app runs on
EXPOSE 3000

# Define the command to run your app
CMD ["npm", "start"]






# ==== Alternatively ===== #

# While alpine:3.9 is a small base image, you might consider using a Node.js-specific image like node:16-alpine to avoid installing Node.js and npm separately. 

# FROM node:16-alpine AS build
# WORKDIR /usr/src/app
# COPY package*.json ./
# RUN npm install

# FROM build
# WORKDIR /app
# COPY . .

# # Optional: Consider removing this line if not required for compatibility
# # RUN export NODE_OPTIONS=--openssl-legacy-provider

# EXPOSE 3000
# CMD ["npm", "start"]
# ======================== #

