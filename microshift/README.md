# MicroShift Sample Application

This repository contains a sample application deployed on Red Hat MicroShift. It demonstrates how to deploy a simple containerized application using MicroShift—a lightweight, edge-optimized distribution of Red Hat OpenShift.

## Introduction to MicroShift

Red Hat MicroShift is designed for environments where resources are limited, such as edge computing and IoT devices. It provides a familiar Kubernetes and OpenShift experience while being optimized for small form factors and challenging networking conditions.

For detailed information on installation, configuration, and best practices, please refer to the official [Red Hat build of MicroShift 4.18 documentation](https://docs.redhat.com/en/documentation/red_hat_build_of_microshift/4.18).

## Key Features

- **Lightweight & Resource Efficient:** Optimized for minimal CPU, memory, and storage usage.
- **Security Focused:** Runs with secure defaults and minimal privileges.
- **Kubernetes Compatible:** Supports standard Kubernetes objects for deployments and services.
- **Edge-Optimized:** Designed to handle intermittent connectivity and remote management.

## Prerequisites

- A running MicroShift cluster (version 4.18 is recommended).
- `oc` configured to interact with your cluster.
- Appropriate access and credentials for pulling container images from your registry.

## Deployment Instructions

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/olilectro/container-demo.git
   cd container-demo/microshift

2. **Deploy the Application:**

   Apply the provided deployment manifest:

   ```bash
   kubectl -n microshift-app apply -f deployment.yaml

3. **Apply Service:**

   In order to access the application within the cluster, we need to deploy a service:

   ```bash
   kubectl apply -f deployment.yaml


4. **Apply Route for external access:**

   If Access from ourside the Cluster is required, you need to deploy the route to the application:

   ```bash
   kubectl apply -f deployment.yaml

## Verification


