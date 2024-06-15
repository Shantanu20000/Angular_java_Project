## $${\color{white} \textbf{Set} \textbf{Up}  \ \textbf{Backend}}$$

This guide provides step-by-step instructions for setting up and running a Spring backend application on Ubuntu.

## Prerequisites
- Ubuntu operating system
- Internet connection

## Instructions

1. **Update apt package index:**

    ```bash
    sudo apt update
    ```

2. **Install OpenJDK 8:**

    ```bash
    sudo apt install openjdk-8-jdk
    ```

3. **Install Maven:**

    ```bash
    sudo apt install maven
    ```
4. **Create Database Connection with backend:**
   ```bash
   vim /src/main/resources/application.properties
   ```
   Note: 1. add db-endpoint 2. username 3. password
   
5. **Build the project using Maven:**

    ```bash
    mvn clean package -Dmaven.test.skip=true
    ```

6. **Run the application:**

    ```bash
    java -jar target/spring-backend-v1.jar
    ```
# $${\color{white} \textbf{Set} \textbf{Up}  \ \textbf{Angular} \ \textbf{Dockerization}}$$
## Notes
- Make sure you have the necessary permissions to execute the commands with `sudo`.
- Ensure that you have the appropriate Java and Maven versions compatible with your Spring application.
- Adjust the file paths and application names as per your project's structure.

Feel free to modify this guide according to your specific requirements.

## Dockerfile
```dockerfile
# Use Ubuntu as base image
FROM ubuntu:latest

# Install dependencies
RUN apt-get update && \
    apt-get install -y openjdk-8-jdk maven && \
    rm -rf /var/lib/apt/lists/*

# Set the working directory in the container
WORKDIR /app

# Copy the Maven project directory into the container
COPY . /app

# Build the Maven project
RUN mvn clean package -Dmaven.test.skip=true

# Expose the port the application runs on
EXPOSE 8080

# Command to run the application
CMD ["java", "-jar", "target/spring-backend-v1.jar"]
```
# Docker Build command fire for create Docker Images.(Note :- Dockerfile should present in same dir where all file of backend is present)
```
docker build -t Shan20000/angular:angular_backend .
```
```
docker images
docker run -td -p 8080:8080 <image_id>
```