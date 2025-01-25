# Use an official Node runtime as a parent image
FROM node:14 AS build

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy the package.json and package-lock.json files to the container
COPY package*.json ./

# Install application dependencies
RUN npm install

# Copy the rest of the application code to the container
COPY . .

#Add multi-stage build
FROM alpine:3.9

WORKDIR /app

RUN apk update && apk add --update nodejs && apk add mongodb mongodb-tools && \
    echo 'http://dl-cdn.alpinelinux.org/alpine/v3.9/main' >> /etc/apk/repositories && \		
    echo 'http://dl-cdn.alpinelinux.org/alpine/v3.9/community' >> /etc/apk/repositories		# Alpine 3.9 Repository


COPY --from=build /usr/src/app /app

# Create MongoDB data directory
RUN mkdir -p /data/db

# Expose necessary ports (MongoDB default: 27017, Node app: 3000)
EXPOSE 27017
EXPOSE 5000

# Start MongoDB and the Node.js application
CMD mongod --bind_ip_all & node server.js

