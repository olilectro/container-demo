# MicroShift Sample Application

This repository contains a sample application deployed on Red Hat MicroShift. It demonstrates how to deploy a simple containerized application using MicroShift—a lightweight, edge-optimized distribution of Red Hat OpenShift.

You might want to change domain names and container images when they are not available. Make sure to configure your microshift cluster according to your `base_domain`.

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

2. **Create a new Namespace for our Application:**
   
    ```bash
    oc create namespace demo

3. **Deploy the Application:**

   Apply the provided deployment manifest:

   ```bash
   oc -n demo apply -f deployment.yaml

4. **Apply Service:**

   In order to access the application within the cluster, we need to deploy a service:

   ```bash
   oc -n demo apply -f service.yaml


4. **Apply Route for external access:**

   If Access from ourside the Cluster is required, you need to deploy the route to the application:

   ```bash
   oc -n demo apply -f route.yaml

## Verification

1. **Verify deployment:**

   Verify yourn new microshift deployment.

   ```bash
   oc -n demo get all
   ```
   If everything is fine, we should see our deployment, replicasets, pods and service.

   ```bash
   NAME                                 READY   STATUS    RESTARTS   AGE
   pod/microshift-app-87657f5b7-lxd8d   1/1     Running   0          6m41s
   pod/microshift-app-87657f5b7-xxk5j   1/1     Running   0          6m44s
   
   NAME                     TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
   service/microshift-app   ClusterIP   10.43.159.57   <none>        8080/TCP   17s
   
   NAME                             READY   UP-TO-DATE   AVAILABLE   AGE
   deployment.apps/microshift-app   2/2     2            2           10m
   
   NAME                                        DESIRED   CURRENT   READY   AGE
   replicaset.apps/microshift-app-776cffb8b5   0         0         0       10m
   replicaset.apps/microshift-app-87657f5b7    2         2         2       6m44s
   ```
2. **Verify routes:**

   ```bash
   oc -n demo get routes/microshift-app
   ```
   The output will show us the host name we are about to use for the connection. It is not possible to use IP adresses, since a route is handled by the loadbalancer/proxy of microshift.

   ```bash
   NAME             HOST                                               ADMITTED   SERVICE          TLS
   microshift-app   microshift-app-demo.apps.microshift.edge.systems   True       microshift-app
   ```

3. **Verify routes:**
   Find out the Cluster Load Balancer external IP.
   ```bash
   oc -n openshift-ingress get service/router-default
   ```
   ```bash
   NAME             TYPE           CLUSTER-IP      EXTERNAL-IP                                 PORT(S)                      AGE
   router-default   LoadBalancer   10.43.209.242   10.1.65.101,10.42.0.2,10.44.0.0,10.88.0.1   80:30531/TCP,443:30293/TCP   3d10h

3. **Make a entry in your DNS Server:**
   Now you can make an entry in your DNS Server for the `HOST`and you should be able to connect to the application using

   https://microshift-app-demo.apps.microshift.edge.systems



Have fun microshifting.

### Further Reading on MicroShift, Containers and OSTree

- [Meet Red Hat Device Edge with MicroShift](https://www.redhat.com/en/blog/meet-red-hat-device-edge-with-microshift)  
  *Red Hat blog post demonstrating the workflow of building a zero-touch Red Hat Device Edge image with MicroShift.*

- [Embedding Red Hat build of MicroShift in a RHEL for Edge image](https://docs.redhat.com/en/documentation/red_hat_build_of_microshift/4.12/html/installing/microshift-embed-in-rpm-ostree)  
  *Official Red Hat documentation on embedding MicroShift into a RHEL for Edge image.*

- [Red Hat build of MicroShift 4.18 Documentation](https://docs.redhat.com/en/documentation/red_hat_build_of_microshift/4.18)  
  *Official documentation covering installation, configuration, and best practices for MicroShift.*

- [MicroShift GitHub Repository](https://github.com/openshift/microshift)  
  *Source code, issue tracking, and community discussions for MicroShift.*

- [Red Hat Enterprise Linux for Edge Documentation](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux_for_edge/)  
  *Guidance on deploying RHEL for Edge, which often leverages OSTree and rpm-ostree technologies.*