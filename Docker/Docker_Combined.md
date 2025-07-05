# Docker Combined Notes

## 001.Docker-what-is-containerization.md
✨ **What is Containerization**
- → To launch a Java software application, traditionally requires a physical/virtual machine, operating system, JDK, and application server (e.g., JBoss, WebLogic).
- → This traditional process can be time-consuming and expensive.
- → Containerization allows creating an abstract representation of all these software components, including the application itself.
- → An `image` is created, which is a lightweight, standalone, executable package that includes everything needed to run a piece of software.
- → Once an image is created, any number of `containers` can be launched from it for the application and its required software, easily and quickly, in any environment.

## 002.Docker-what-is-docker.md
✨ **What is Docker**
- → Docker is a containerization tool or framework that allows packaging applications once and running them anywhere.
- → It implements the Open Container Initiative (OCI) standard.
- → When an application is Dockerized, it no longer exists as traditional artifacts (WAR, JAR, DLL, Django code), but as a `Docker image`.
- → A Docker image contains the entire infrastructure information along with the application itself.
- → From a Docker image, any number of `containers` can be launched on any operating system that has Docker installed.
- → Components can be put into different images (e.g., DB server, web server, application), and separate containers can be launched from these images to communicate with each other.

## 003.Docker-what-are-the-docker-components-and-workflow.md
✨ **Docker Components and Workflow**
- → **Docker Repository/Registry:** Centralized storage for all Docker images.
- → **Docker Host:** The machine where the Docker engine is installed.
- → **Docker Client:** Provides the ability to run commands against the Docker engine.
- → **Typical Workflow:**
    - → When a `docker pull` command is executed, the Docker engine first checks for the image locally.
    - → If not found locally, it pulls the image from the central Docker repository and stores it locally.
    - → When a `docker run` command is executed, the Docker engine uses the locally available image to launch a container.
    - → Users can create their own Docker images from their applications, build them, and push them to the central Docker repository for others to use.

## 004.Docker-why-docker.md
✨ **Why Docker**
- → Docker uses the concept of `image layers` and `overlay filesystem`.
- → A Docker image is made up of multiple layers.
- → It uses a union filesystem to integrate these layers together.
- → When an image is first pulled, it pulls several layers, starting with the base kernel, and then merges/overlays them.
- → Subsequent pulls of the same image only pull the layers that have changed.
- → **Example:** If only the application JAR/WAR has changed, only that specific layer will be pulled and overlaid on top of the existing layers.
- → This improves performance and reduces network bandwidth.

## 005.Docker-what-are-some-of-the-docker-commands-you-have-used.md
✨ **Common Docker Commands**
- → `docker images`: Lists all Docker images on the machine.
- → `docker pull <image_name>`: Pulls an image from the central Docker repository.
- → `docker run <image_name>`: Launches a container from an image (pulls if not local).
- → `docker ps`: Lists all running containers.
- → `docker start <container_id/name>`: Starts a stopped container.
- → `docker stop <container_id/name>`: Stops a running container.
- → `docker build -t <image_name> .`: Builds a Docker image from a Dockerfile.
- → `docker push <image_name>`: Pushes a local image to a central Docker registry (e.g., Docker Hub).
- → `docker exec -it <container_id/name> <command>`: Executes a command inside a running container.
- → `docker search <term>`: Searches for images on the central Docker repository.
- → `docker attach <container_id/name>`: Attaches to a detached running container.
- → `docker commit <container_id> <new_image_name>`: Creates a new image from a container's changes.
- → `docker rm <container_id/name>`: Removes a container.
- → `docker rmi <image_id/name>`: Deletes an image.
- → `docker image prune`: Deletes unused images.
- → `docker container prune`: Deletes unused containers.

## 006.Docker-what-are-docker-volumes.md
✨ **What are Docker Volumes**
- → When a Docker container is deleted, the application data on that container is also deleted or lost.
- → To persist this data onto the host machine where Docker is running, `Bind mounts` or `Volumes` can be used.
- → Bind mounts and volumes are essentially folders on the host machine that are bound or mounted to a folder inside the container.
- → This ensures that data is stored on the host machine instead of within the container itself.
- → Even if the container is deleted, the data remains on the host machine.
- → The next time a new container is launched from the same Docker image, it can continue to use and update the data on the host machine through this volume or bind mount.

## 007.Docker-volumes-vs-bind-mounts.md
✨ **Volumes vs Bind Mounts**
- → **Bind Mounts:**
    - → Can be any folder on the host machine.
    - → Not managed by Docker.
    - → Any process on the host machine can have direct access to these folders.
- → **Volumes:**
    - → Managed objects controlled by Docker.
    - → Live under a specific directory within Docker's managed space.
    - → No other process on the host machine will have direct access to these volumes, providing better isolation and management.

## 008.Docker-how-did-you-dockerize-your-application.md
✨ **How to Dockerize a Spring Boot Application**
- → **Step 1: Create a Dockerfile:** Create a file named `Dockerfile` (no extension) in the project root.
- → **Step 2: Specify Base Image:** The first instruction in the Dockerfile specifies the base image (e.g., `openjdk:17-jdk-slim`).
- → **Step 3: Add JAR File:** Copy the Spring Boot executable JAR file (typically found in the `target` or build directory) into the Docker image.
    - → **Example:** `COPY target/your-application.jar app.jar`
- → **Step 4: Define Entry Point:** The last instruction defines the entry point, which is the command to be run as soon as the container starts.
    - → This command typically executes the JAR file.
    - → **Example:** `ENTRYPOINT ["java", "-jar", "app.jar"]`
- → This setup ensures that when a container is launched from this image, the Spring Boot application inside it will automatically start.

## 009.Docker-what-is-docker-compose.md
✨ **What is Docker Compose**
- → Docker Compose is a tool used to define and run multi-container Docker applications.
- → It allows bringing up or taking down multiple Docker containers that depend on each other in one shot.
- → **Example:** If a product microservice uses a coupon microservice, and both depend on a MySQL database, all three can be defined and managed together.
- → **Docker Compose File:** Services (containers) are defined in a `docker-compose.yml` file.
    - → This file specifies container information such as the image to use, restart policies, exposed ports, and environment variables.
    - → Services can declare dependencies on each other.
- → **Workflow:** When `docker compose up` is run using this file:
    - → The Docker MySQL container will be launched first.
    - → Then the Coupon microservice container will be launched.
    - → Finally, the Product microservice container will be launched, as it depends on the other two.
- → Docker Compose simplifies the management of interconnected containers, allowing them to be launched and taken down with single commands.
