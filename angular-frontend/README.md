## $${\color{white} \textbf{Set} \textbf{Up}  \ \textbf{Angular} \ \textbf{Frontend}}$$

This guide provides step-by-step instructions for setting up an Angular project from scratch on an Ubuntu machine.
### Install AWS CLI 
````
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
````
````
aws configure
````

## Step 1: Install Node.js and npm

```bash
sudo apt update
sudo apt install nodejs npm
```
## Step 2: Check Node.js and npm versions

```bash
node -v
npm -v
```
## Step 3: Install Angular CLI globally
```bash
sudo npm install -g @angular/cli@14.2.1
```
## Step 4: Verify Angular CLI installation
```bash
ng version
```
## Step 5: Connect to backend server
````
cd src/app/services
sudo nano worker.service.ts
````
Note: add public-ip of backend server

## Step 6: Install project dependencies (if needed)

```bash
npm install
ng build 
sudo ng serve --host 0.0.0.0 --port=80
```

![ng-serve-host](https://github.com/abhipraydhoble/Project-Angular-App/assets/122669982/6e07ffc0-6c54-403c-9e86-62cd85f898fa)


## Step7: Go to browser and hit public-ip of frontend instance


![angular-ip](https://github.com/abhipraydhoble/Project-Angular-App/assets/122669982/bac432e1-9f04-4b7d-81c9-8ed8e879793b)

# $${\color{white} \textbf{Set} \textbf{Up}  \ \textbf{Angular} \ \textbf{Dockerization}}$$

This guide provides step-by-step instructions for Dockerizing an existing Angular project. Docker allows you to package your Angular application into a container, making it portable and easily deployable across different environments.

## Prerequisites

- Docker installed on your system. You can download and install Docker Desktop from [here](https://docs.docker.com/engine/install/).

## Dockerfile Setup

Create a `Dockerfile` in the root directory of your Angular project with the following content:

```Dockerfile
# Use official Node.js image as the base image
FROM node:14-alpine as build

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy package.json and package-lock.json (if available)
COPY package*.json ./

# Install project dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the Angular application
RUN npm run build 

# Use NGINX as the production server
FROM nginx:alpine

# Copy the built artifact from the previous stage to NGINX web server directory
COPY --from=build /usr/src/app/dist/angular-frontend /usr/share/nginx/html

# Expose port 80 to the outside world
EXPOSE 80

# Start NGINX server when the container starts
CMD ["nginx", "-g", "daemon off;"]
```
# In frontend we have mention our backend ip or service name in bellow file (Note:- At the place of localhost)
```
vim ./angular-frontend/src/app/services/worker.service.ts
```
# Docker Build command fire for create Docker Images.(Note :- Dockerfile should present in same dir where all file of backend is present)
```
docker build -t Shan20000/angular:angular_frontend .
```
```
docker images
docker run -td -p 80:80 <image_id>
```