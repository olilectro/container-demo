# Nginx with Red Hat Universal Base Image Example Using Podman

This repository demonstrates how to work with Red Hat’s Universal Base Images (UBI) in the context of running an Nginx web server using Podman instead of Docker. The example uses a ubi9 image, and the concepts apply equally to UBI9 catalog images. This guide is designed for students and newcomers to containerization, explaining each step and command in the Dockerfile (adapted for Podman).

## Overview

The provided Dockerfile (which is compatible with Podman) performs the following tasks:
- **Selects a Base Image:** Uses a UBI image that comes pre-configured with Nginx.
- **Sets Environment Variables:** Defines an environment variable for the Nginx version.
- **Adds Application Files:** Copies Nginx configuration files and HTML content from the local build context into the container.
- **Runs Nginx:** Starts the Nginx service in the foreground so that the container continues running.

## Detailed Explanation

### Base Image

```dockerfile
FROM registry.access.redhat.com/ubi9/nginx-120
```

#### What it does:
This line instructs Podman to use the ubi9/nginx-120 image from Red Hat’s container registry as the base image.
#### Why it matters:
Red Hat’s Universal Base Images are designed for security, consistency, and compliance.
They come with the necessary support for critical edge applications.
#### Learn more:
[Red Hat UBI Overview](https://developers.redhat.com/products/ubi/overview)

```Environment Variable
ENV NGINX_VERSION=1.20
```
#### What it does:
Sets an environment variable NGINX_VERSION to specify the version of Nginx in use.
#### Why it matters:
Environment variables simplify updates and configuration management while documenting the intended version of Nginx running inside the container.

```Adding Application Sources
# Add application sources
ADD test-app/nginx.conf "${NGINX_CONF_PATH}"
ADD test-app/nginx-default-cfg/*.conf "${NGINX_DEFAULT_CONF_PATH}"
ADD test-app/nginx-cfg/*.conf "${NGINX_CONFIGURATION_PATH}"
ADD test-app/*.html ./
```
#### What it does:
Copies local application files (configuration files and HTML documents) into the container at paths defined by environment variables (NGINX_CONF_PATH, NGINX_DEFAULT_CONF_PATH, NGINX_CONFIGURATION_PATH).
#### Why it matters:
This method allows you to customize Nginx configuration without altering the base image. It is an excellent example of container layering and the reuse of standard images.
#### Learn more:
[Understanding Dockerfile Instructions](https://docs.docker.com/reference/dockerfile/)

```Running Nginx
# Run script uses standard ways to run the application
CMD nginx -g "daemon off;"
```
#### What it does:
The CMD instruction tells Podman to start Nginx in the foreground.
#### Why it matters:
Running Nginx in the foreground prevents the container from exiting immediately after launch. The daemon off; flag is a standard practice in containerized environments.

#### Learn more:
[Nginx Official Documentation](https://nginx.org/en/docs/)

## How to Build and Run with Podman
1.	Build the Image:
Open a terminal in the directory containing the Dockerfile and run:

podman build -t my-nginx .


2.	Run the Container:
Once the build is complete, start a container with:

```podman run -p 8080:8080 nginx-edge```

This command maps port 8080 of the container to port 8080 on your host machine, allowing you to access the Nginx web server via a browser.

Additional Resources
- Red Hat Containers Documentation:
For more information about Red Hat container images and UBI, visit [Red Hat Containers](https://access.redhat.com/containers).
- Podman Documentation:
Learn more about Podman and its features in the [Podman Official Documentation](https://podman.io/).

Summary

This example shows how to leverage a Red Hat UBI image to create a containerized Nginx server using Podman. By adding custom configuration files and static HTML content, you can easily tailor the container to your requirements. The approach outlined here is applicable for secure and supported containers on critical edge infrastructure.

Happy containerizing with Podman! :shipit:


## test-app Overview

- NGINX_CONF_PATH
This is used to specify where the main NGINX configuration file (typically your custom nginx.conf) should be placed inside the container.
- NGINX_DEFAULT_CONF_PATH
This variable designates the directory for “default” configuration snippets that might be used to override or supplement the main configuration. They might be used for setting up default server blocks or additional parameters.
- NGINX_CONFIGURATION_PATH
This one typically points to where extra or custom configuration files (for example, additional virtual host configurations) should be stored.