# Bincom DevOps Assignment: Docker Containerization and Deployment

## Project Overview

This project demonstrates the containerization and deployment of a web application using Docker, Docker Compose, Docker Hub, and a remote server.

The application is a static website served by Nginx inside a Docker container. Docker Compose manages the web service alongside a MySQL database service.

This project demonstrates practical DevOps skills, including building Docker images, running containers, managing multiple services, publishing images to a container registry, and deploying a containerized application.

## Project Objectives

- Build a custom Docker image for a web application.
- Run the website inside a Docker container.
- Use Docker Compose to manage a web service and a MySQL database.
- Publish the custom Docker image to Docker Hub.
- Pull the image onto a remote server.
- Deploy the application and access it through a web browser.

## Technologies Used

- Docker
- Docker Compose
- Nginx
- MySQL
- Docker Hub
- Ubuntu Server
- AWS EC2 

## Project Structure

```text
bincom-docker-assignment/
├── Dockerfile
├── compose.yaml
├── index.html
├── .dockerignore
├── README.md
└── screenshots/
```

## Dockerfile

The Dockerfile defines how the custom web application image is built.

It uses the lightweight Nginx Alpine image as its base and copies the HTML website into Nginx's default web directory.

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

## Building the Docker Image

To build the custom image, run the following command from the project directory:

```bash
docker build -t bincom-web:1.0 .
```

Verify that the image has been created:

```bash
docker images
```

## Running the Web Application

The web application can be run independently of Docker Compose:

```bash
docker run -d --name bincom-web-container -p 8080:80 bincom-web:1.0
```

Open the following address in your browser:

http://localhost:8080

Check the running container:

```bash
docker ps
```

## Docker Compose Configuration

Docker Compose manages the web application and MySQL database as separate services.

The configuration includes:

- **Web service:** Builds the website image using the Dockerfile in the current directory.
- **Port mapping:** Maps port 8081 on the host to port 80 inside the web container.
- **Database service:** Runs MySQL 8 and creates a database named `appdb`.
- **Persistent storage:** Uses the `dbdata` named volume to preserve MySQL data when the database container is recreated.
- **Service dependency:** Uses `depends_on` to start the database service before starting the web service.

### Docker Compose File

The `compose.yaml` file contains:

```yaml
services:
  web:
    build: .
    ports:
      - "8080:80"
    depends_on:
      - db

  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: appdb
    volumes:
      - dbdata:/var/lib/mysql

volumes:
  dbdata:
```

### Starting the Services

Run the following command from the project directory:

```bash
docker compose up -d --build
```

This builds the web image and starts the web and database services in the background.

Check the services:

```bash
docker compose ps
```

View the application logs:

```bash
docker compose logs
```

To view the database logs:

```bash
docker compose logs db
```

### Accessing the Website

Open the following address in your browser:

http://localhost:8080

### Stopping the Services

To stop and remove the Compose containers and network, run:

```bash
docker compose down
```

The named `dbdata` volume is retained by this command.

**Note:** The website is a static HTML application. Although the MySQL database runs as a separate service, the website does not currently connect to the database.

## Docker Hub Repository

The custom Docker image is published to Docker Hub.

**Repository:** [asiwaju007/bincom-web](https://hub.docker.com/r/asiwaju007/bincom-web)

**Image:** `asiwaju007/bincom-web:1.0`

### Pulling the Image

To download the published image:

```bash
docker pull asiwaju007/bincom-web:1.0
```

### Running the Published Image

```bash
docker run -d --name bincom-web-container -p 8080:80 asiwaju007/bincom-web:1.0
```

Open the website at:

http://localhost:8080

## Remote Server Deployment

The custom Docker image can be deployed to a separate Ubuntu server.

The deployment process involves:

1. Provisioning an Ubuntu server.
2. Installing Docker Engine.
3. Pulling the published image from Docker Hub.
4. Running the container on the server.
5. Configuring the firewall or security group to permit HTTP traffic.
6. Accessing the application through a web browser.

### Pull the Image on the Server

```bash
docker pull asiwaju007/bincom-web:1.0
```

### Run the Container on the Server

```bash
docker run -d --name bincom-web-container -p 80:80 asiwaju007/bincom-web:1.0
```

Access the application using:

`http://YOUR_SERVER_PUBLIC_IP`
http://3.9.164.233/

**Security note:** The MySQL root password in the Compose example is `example`, which is suitable only for a disposable demonstration environment. Do not use this password in production. Use a strong password and keep database credentials out of public repositories. Do not expose MySQL port 3306 publicly.

## Screenshots and Evidence

Screenshots can be stored in the `screenshots/` directory to demonstrate:

- The website running in a browser.
- The web and database containers running through Docker Compose.
- The Docker image published on Docker Hub.
- The application deployed on the remote server.

## Learning Outcomes

This project provided practical experience with:

- Writing a Dockerfile.
- Building and tagging Docker images.
- Running and inspecting containers.
- Using Docker Compose to manage multiple services.
- Configuring port mappings.
- Managing persistent storage with Docker volumes.
- Publishing and pulling images using Docker Hub.
- Deploying a containerized application to a remote server.
- Documenting a DevOps project using GitHub.

## Author

**Adedoyin Emmanuel**

Cloud Engineering and DevOps Learner

## Project Status

Completed as a practical Docker containerization and deployment assignment for Bincom Academy.
