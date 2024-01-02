# Socket Server

This repository contains the Socket Server for the CarGator application, providing real-time communication capabilities between the CarGator components.

## Running the Socket Server Locally

### Prerequisites

- Node.js (with npm) installed on your machine.

### Installation

1. Clone the Socket Server repository:

    ```bash
    $ git clone https://github.com/cargator/socket-server.git
    $ cd socket-server
    ```

2. Install dependencies:

    ```bash
    $ npm install
    ```

3. Configuration:

    - Create a `.env` file in the root directory by copying the contents from `sample.env`.
    - Customize the variables in the `.env` file based on your specific requirements.

### Start the Socket Server

Start the Socket Server by running the following command:

```bash
$ npm run start:dev
```

# Dockerfile:

Create a file named Dockerfile in the root of your project. This file contains instructions for building your Docker image.
```bash
FROM node:18-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install


# Bundle app source
COPY . . 

RUN npm run build

CMD npm run start
```
## Build the Docker Image:
Open a terminal in the directory containing your Dockerfile and run the following command to build the Docker image:
```bash
docker build -t your-image-name .
```

## Run the Docker Container:
Once the image is built, you can run a container:

```bash
docker run -p 3000:3000 -d your-image-name
```
This command maps port 3000 on your host machine to port 3000 on the container.

## Deployment Steps:
1. Build and Push Docker Image to GCR:

```bash
#Build the Docker image locally
docker build -t gcr.io/YOUR_PROJECT_ID/backend-app:v1 .

#Authenticate Docker to push images to GCR
gcloud auth configure-docker

#Push the Docker image to GCR
docker push gcr.io/YOUR_PROJECT_ID/backend-app:v1
```
Replace YOUR_PROJECT_ID with your actual GCP project ID.

2. Create Compute Engine Instance:
   Use the GCP Console or command line to create a Compute Engine instance.
   Specify the Docker image from GCR in the instance settings.
   
4. Access the Deployed Backend:
   Visit the external IP address of your Compute Engine instance in a web browser or use tools like curl or Postman to make requests.


Certainly! Here's the Docker Compose setup for MongoDB and a Node.js backend in document format:

# Pulling MongoDB Docker Image from Docker Hub

## Overview

This document provides step-by-step instructions on how to pull the official MongoDB Docker image from Docker Hub.

## Prerequisites

Docker installed on your machine.

## Steps

### 1. Open a Terminal

  Open your terminal or command prompt to execute Docker commands.

### 2. Pull MongoDB Docker Image

  Use the following command to pull the latest MongoDB Docker image:
  ```bash
  docker pull mongo
  ```

### 3. Verify Image

  After the image is downloaded, you can verify its presence by running:
  ```bash
  docker images
  ```
This should display the newly pulled MongoDB image.

### 4. Run MongoDB Container

To run a MongoDB container, you can use the following command:

```bash
docker run -d -p 27017:27017 --name mongodb mongo
```

Adjust the port mappings and container name as needed for your specific setup.

### 5. Verify MongoDB Container

Check if the MongoDB container is running:

```bash
docker ps
```

This should display information about the running MongoDB container.

# Docker Compose Setup for MongoDB and Node.js Backend

## Overview

This document provides instructions for using Docker Compose to set up containers for MongoDB and a Node.js backend. Docker Compose allows you to define and run multi-container Docker applications.

## Prerequisites

1.Docker installed on your machine.

2.Node.js application with a Dockerfile.

3.Basic understanding of Docker Compose.

## Steps

### 1. Create Docker Compose File

Create a file named docker-compose.yml in your project's root directory with the following content:
```bash
version: '3'
services:
  mongodb:
    image: mongo
    ports:
      - "27017:27017"
    volumes:
      - mongodb_data:/data/db
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=adminpassword

  backend:
    build:
      context: .
      dockerfile: Dockerfile   # Replace with the path to your Node.js Dockerfile
    ports:
      - "3000:3000"
    depends_on:
      - mongodb
    environment:
      - MONGODB_URI=mongodb://admin:adminpassword@mongodb:27017/yourdbname   # Change yourdbname to your actual database name

volumes:
  mongodb_data:
```

### 2. Adjust Configuration

1.Replace Dockerfile with the actual path to your Node.js Dockerfile.
2.Change yourdbname in the MONGODB_URI to your actual MongoDB database name.

### 3. Build and Run

Open a terminal in the directory containing the docker-compose.yml file and run:

```bash
docker-compose up
```

To run in the background:

```bash
docker-compose up -d
```

### 4. Verify Services

Check if both services are running:

```bash
docker-compose ps
```

Access your Node.js application at http://localhost:3000 and verify the MongoDB connection.

### 5. Stop Services

To stop the services, use:

```bash
docker-compose down
```
