
# Table of contents 
- [Table of contents](#table-of-contents)
- [What is Kubernetes](#what-is-kubernetes)
- [Why is Kubernetes needed](#why-is-kubernetes-needed)
- [Benefits of Kubernetes](#benefits-of-kubernetes)
- [Use cases](#use-cases)
- [Success stories](#success-stories)
- [Kubernetes architecture (include a diagram)](#kubernetes-architecture-include-a-diagram)
- [The cluster setup](#the-cluster-setup)
  - [What is a cluster](#what-is-a-cluster)
  - [Master vs worker nodes](#master-vs-worker-nodes)
    - [Master Node (Control Plane)](#master-node-control-plane)
      - [Responsibilities of the Master Node:](#responsibilities-of-the-master-node)
    - [Worker Nodes](#worker-nodes)
      - [Responsibilities of Worker Nodes:](#responsibilities-of-worker-nodes)
  - [Pros and cons of using managed service vs launching your own](#pros-and-cons-of-using-managed-service-vs-launching-your-own)
    - [Managed Kubernetes Service](#managed-kubernetes-service)
    - [Self-Managed Kubernetes](#self-managed-kubernetes)
  - [Control plane vs data plane](#control-plane-vs-data-plane)
    - [Control Plane](#control-plane)
    - [Data Plane](#data-plane)
    - [Summary](#summary)
    - [Control Plane](#control-plane-1)
    - [Data Plane](#data-plane-1)
- [Kubernetes objects](#kubernetes-objects)
  - [Research the most common ones, e.g. Deployments, ReplicaSets, Pods](#research-the-most-common-ones-eg-deployments-replicasets-pods)
  - [What does it mean a pod is "ephemeral"](#what-does-it-mean-a-pod-is-ephemeral)
- [How to mitigate security concerns with containers](#how-to-mitigate-security-concerns-with-containers)
    - [Best Practices for Container Security](#best-practices-for-container-security)
- [Maintained images](#maintained-images)
  - [What are they](#what-are-they)
  - [Pros and cons of using maintained images for your base container images](#pros-and-cons-of-using-maintained-images-for-your-base-container-images)
    - [Pros of Using Maintained Images](#pros-of-using-maintained-images)
    - [Cons of Using Maintained Images](#cons-of-using-maintained-images)



# What is Kubernetes

Kubernetes is an open-source platform for automating the deployment, scaling, and management of containerized applications.
<br>
it has become the industry standard for container orchestration, providing a robust system to handle complex, distributed applications across multiple hosts or cloud environments.

# Why is Kubernetes needed
- Automated Scaling: Scales applications up/down based on demand, optimizing resource use.
- Self-Healing: Automatically restarts or reschedules failed containers, ensuring reliability.
- Simplified Deployments: Automates deployment, updates, and rollbacks, reducing downtime.
- Load Balancing: Distributes traffic across containers, maintaining performance.
- Resource Efficiency: Optimizes resource usage by scheduling workloads onto available nodes.
- Multi-Cloud Compatibility: Runs consistently across cloud providers and on-premises.
- Environment Consistency: Ensures applications run uniformly from development to production.
- Microservices Management: Manages independent services, ideal for distributed architectures.

# Benefits of Kubernetes


Scalability: Automatically adjusts resources to meet demand.
<br>
Reliability: Self-healing ensures minimal downtime.
<br>
Efficient Resource Use: Optimizes CPU, memory, and storage across containers.
<br>
Simplified Deployment: Automates updates, rollbacks, and scaling.
<br>
Portability: Runs across clouds and on-premises seamlessly.
<br>
Microservices Support: Ideal for managing distributed, independent services.
<br>
Consistent Environments: Ensures uniformity from development to production.

# Use cases 
Large e-commerce site with different services (like product catalog, payment processing, and user authentication) each running as microservices in separate containers. Kubernetes could manage this distributed application by:

Ensuring each microservice runs in a designated number of replicas for redundancy.
Scaling the payment service automatically during high-traffic periods.
Restarting any failed service to ensure uninterrupted service.
Managing storage for database-backed services.
# Success stories
**Pinterest**
Challenge: Pinterest experiences extreme traffic spikes, especially during events like holidays, requiring flexible scaling.
<br>
Solution: Kubernetes allowed Pinterest to dynamically scale services up and down in response to user demand, optimizing resource costs and enhancing user experience during peak times.
<br>

**Airbnb**
Challenge: With a complex microservices architecture, Airbnb needed to scale services efficiently, manage rapid deployments, and reduce system downtime.
<br>
Solution: Kubernetes enabled Airbnb to automate scaling and load balancing, and improve the speed of deployments. This streamlined resource usage and allowed the team to innovate faster.

# Kubernetes architecture (include a diagram)
![alt text](images/kubearch.png)

# The cluster setup
## What is a cluster
A collection of connected computers (often called nodes) that work together to function as a single, cohesive system. 
<br>
These nodes share resources, distribute workloads, and allow applications to run across multiple machines

## Master vs worker nodes

### Master Node (Control Plane)
The master node, or control plane, is responsible for managing the overall state and coordination of the cluster. Key components include:

1. **API Server**: Acts as the main interface to the cluster. All components communicate through it, and it handles requests (e.g., from kubectl).
   
2. **Scheduler**: Decides which node to assign to each new pod based on available resources, policies, and requirements.

3. **Controller Manager**: Manages the cluster's desired state by running controllers that handle tasks like scaling, maintaining pod states, and tracking node health.

4. **etcd**: A distributed key-value store that serves as the cluster’s “source of truth,” storing all configuration data and state information about the cluster.

5. **Cloud Controller Manager** (optional): Integrates with cloud providers to manage resources like load balancers, storage, and networking for Kubernetes.

#### Responsibilities of the Master Node:
- **Orchestration**: Oversees the scheduling of pods and workloads.
- **State Management**: Ensures that the cluster’s desired state matches the actual state (self-healing).
- **Monitoring and Maintenance**: Monitors node and application health, initiating action if issues arise.
- **Scaling and Updates**: Manages scaling (adding/removing pods) and rolling updates.

### Worker Nodes
Worker nodes run the actual applications within containers. Each worker node is responsible for running and managing the lifecycle of the containers assigned to it. Key components include:

1. **Kubelet**: An agent that ensures the containers in a pod are running as expected. It communicates with the API server on the master node.

2. **Kube-Proxy**: Manages network rules, facilitating communication between pods and services inside and outside the cluster.

3. **Container Runtime** (e.g., Docker, containerd): Runs the containers that make up each pod.

#### Responsibilities of Worker Nodes:
- **Run Application Workloads**: Hosts the pods, which contain containers running applications.
- **Resource Management**: Manages CPU, memory, and storage for the pods on the node.
- **Networking**: Facilitates communication within and outside the cluster using the kube-proxy component.


## Pros and cons of using managed service vs launching your own

### Managed Kubernetes Service

**Pros**:
- **Easy Setup & Maintenance**: Quick to set up with automated updates and security.
- **Reliability**: High availability and integration with cloud services.
- **Scalability**: Simplified auto-scaling and resource management.

**Cons**:
- **Less Customizable**: Limited access to low-level configurations.
- **Potentially Higher Costs**: May cost more than self-managed setups.
- **Vendor Lock-In**: Dependence on a single cloud provider.

### Self-Managed Kubernetes

**Pros**:
- **Full Control**: Complete customization and flexibility.
- **Lower Cost Potential**: Can be more cost-effective at scale.
- **Infrastructure Flexibility**: Suitable for on-premises, hybrid, or multi-cloud environments.

**Cons**:
- **Higher Maintenance**: Requires significant expertise and ongoing management.
- **Security Burden**: Security patches and updates are the organization’s responsibility.
- **Reliability Challenges**: Ensuring high availability is more complex.

## Control plane vs data plane

In Kubernetes (and networking), the **control plane** and **data plane** are two distinct components that play different roles:

### Control Plane
- **Purpose**: Manages and orchestrates the cluster, maintaining the overall state of applications and resources.
- **Key Components**:
  - **API Server**: The main communication hub for the cluster, interacting with kubectl and other components.
  - **Scheduler**: Assigns pods to specific nodes based on resources and policies.
  - **Controller Manager**: Manages controllers that ensure the desired state (e.g., scaling, health checks).
  - **etcd**: Stores cluster state data.
- **Function**: Oversees the deployment and scaling of applications, monitors health, and makes scheduling decisions.

### Data Plane
- **Purpose**: Executes and runs application workloads.
- **Key Components**:
  - **Nodes**: Physical or virtual machines running the actual applications in containers.
  - **Kubelet**: Ensures containers in pods are running correctly.
  - **Kube-Proxy**: Manages networking for communication within the cluster.
- **Function**: Executes instructions from the control plane, runs application containers, manages traffic, and maintains resource usage on nodes.

### Summary
- **Control Plane**: Manages the cluster's desired state and scheduling.
- **Data Plane**: Runs applications and handles data traffic within the cluster.In Kubernetes (and networking), the **control plane** and **data plane** are two distinct components that play different roles:

### Control Plane
- **Purpose**: Manages and orchestrates the cluster, maintaining the overall state of applications and resources.
- **Key Components**:
  - **API Server**: The main communication hub for the cluster, interacting with kubectl and other components.
  - **Scheduler**: Assigns pods to specific nodes based on resources and policies.
  - **Controller Manager**: Manages controllers that ensure the desired state (e.g., scaling, health checks).
  - **etcd**: Stores cluster state data.
- **Function**: Oversees the deployment and scaling of applications, monitors health, and makes scheduling decisions.

### Data Plane
- **Purpose**: Executes and runs application workloads.
- **Key Components**:
  - **Nodes**: Physical or virtual machines running the actual applications in containers.
  - **Kubelet**: Ensures containers in pods are running correctly.
  - **Kube-Proxy**: Manages networking for communication within the cluster.
- **Function**: Executes instructions from the control plane, runs application containers, manages traffic, and maintains resource usage on nodes.

# Kubernetes objects

## Research the most common ones, e.g. Deployments, ReplicaSets, Pods

In Kubernetes, several core objects are used to manage applications and their resources. Here are the most common Kubernetes objects:

1. **Pod**
-  The smallest and simplest Kubernetes object, a Pod represents a single instance of a running application. It can contain one or more containers that share storage, networking, and lifecycle.
- **Use Case**: Running a single containerized application or multiple tightly coupled containers that need to share resources.

1. **Deployment**
- A Deployment manages a set of identical Pods, ensuring the specified number of replicas are running and managing updates to the application. It provides declarative updates to Pods.
- **Use Case**: Simplifying the process of deploying and scaling applications, rolling out updates, and managing rollback capabilities.

1. **ReplicaSet**
-  Ensures that a specified number of Pod replicas are running at any given time. While often used indirectly through Deployments, ReplicaSets can also be used independently.
- **Use Case**: Maintaining a stable set of identical Pods during scaling operations.

1. **Service**
-An abstraction that defines a logical set of Pods and a policy to access them. Services enable communication between different components and provide load balancing.
- **Use Case**: Exposing an application running on a set of Pods to other services or external traffic.

1. **Namespace**
- A way to partition resources within a Kubernetes cluster. Namespaces allow for organizing and managing resources effectively, especially in multi-tenant environments.
- **Use Case**: Creating isolated environments for different teams or applications within the same cluster.

1. **ConfigMap**
- Allows you to decouple configuration artifacts from image content to keep containerized applications portable. ConfigMaps store non-sensitive configuration data as key-value pairs.
- **Use Case**: Managing application configuration that can change independently of the application code.

1. **Secret**
Similar to ConfigMaps, but used for storing sensitive information such as passwords, tokens, or SSH keys, with additional encoding to protect the data.
- **Use Case**: Managing sensitive information securely within your application.

8. **Volume**
- Represents storage that can be used by Pods, providing persistent storage beyond the lifecycle of individual Pods. Various types of volumes are available (e.g., emptyDir, persistentVolumeClaim).
- **Use Case**: Storing data that needs to persist even when Pods are deleted or recreated.

1. **PersistentVolume (PV) & PersistentVolumeClaim (PVC)**
- **PersistentVolume (PV)**: A piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes.
- **PersistentVolumeClaim (PVC)**: A request for storage by a user. PVCs enable Pods to request storage resources.
- **Use Case**: Providing durable storage that persists beyond the life of Pods.

1. **Job and CronJob**
- **Job**: A controller that creates one or more Pods and ensures that a specified number of them successfully terminate. Jobs are used for batch processing or one-off tasks.
- **CronJob**: Similar to Jobs but allows for running jobs on a scheduled basis (e.g., daily, weekly).
- **Use Case**: Performing scheduled or batch processing tasks.

These objects form the foundation of Kubernetes 

## What does it mean a pod is "ephemeral"
It means that the Pod is temporary and has a limited lifespan.
<br>
For example, a Pod might be created to handle a specific task and terminated once the task is completed.

# How to mitigate security concerns with containers
To mitigate security concerns with containers, consider the following strategies:

### Best Practices for Container Security

1. **Use Trusted Images**: Start with official or trusted base images and regularly scan for vulnerabilities.
2. **Minimize Attack Surface**: Use minimal base images and remove unnecessary packages.
3. **Implement Least Privilege**: Run containers as non-root users and limit privileges using Linux capabilities.
4. **Network Security**: Isolate containers with network policies and control traffic with firewalls.
5. **Secure Registries**: Use private registries with authentication and enable image vulnerability scanning.
6. **Regular Updates**: Keep container images up-to-date with security patches and automate CI/CD pipelines for updates.
7. **Runtime Monitoring**: Utilize tools like Falco for real-time monitoring and logging of container activities.
8. **Configuration Management**: Enforce security configurations consistently with management tools.
9. **Secrets Management**: Use secure tools for managing sensitive information, avoiding hardcoding secrets.
10. **Resource Limits**: Set CPU and memory limits to prevent denial-of-service attacks.
11. **Image Signing**: Sign images to ensure only verified images are deployed.
12. **Security Policies**: Define and enforce security policies, regularly assess compliance.
13. **Team Education**: Provide training on container security best practices to developers and operations teams.

# Maintained images
## What are they
They are container images that are regularly updated, monitored, and supported by their maintainers. These images are essential for ensuring security, stability, and compatibility in containerized applications.

## Pros and cons of using maintained images for your base container images

Here are the pros and cons of using maintained images as your base container images:

### Pros of Using Maintained Images

1. **Security**:
   - Regular updates and patches address vulnerabilities, enhancing overall security.
   - Security scans help identify and mitigate known issues.

2. **Reliability**:
   - Maintained images are often tested and verified, reducing the likelihood of encountering bugs.
   - Consistent support ensures stability in production environments.

3. **Performance Improvements**:
   - Ongoing optimizations can lead to better performance and resource utilization.
   - Updates may include performance enhancements and feature upgrades.

4. **Ease of Use**:
   - Well-documented images provide clear instructions, reducing setup time.
   - Support communities or forums can offer assistance, helping developers troubleshoot issues.

5. **Version Control**:
   - Maintained images usually follow versioning practices, allowing easy tracking of changes and updates.
   - Users can choose specific versions based on compatibility and stability needs.

### Cons of Using Maintained Images

1. **Dependency on Maintainers**:
   - Reliance on third-party maintainers means users must trust their ability to keep the images updated.
   - If maintainers abandon a project, users may be left with outdated images.

2. **Potential for Breaking Changes**:
   - Regular updates may introduce breaking changes or incompatibilities, requiring developers to adapt their applications.
   - Users need to monitor changes carefully and test updates before deployment.

3. **Resource Consumption**:
   - Frequent updates may require additional resources for testing and integration, especially in CI/CD pipelines.
   - Maintaining the latest versions might lead to increased image sizes if not managed properly.

4. **Limited Customization**:
   - Maintained images may not be as flexible as custom-built images, limiting the ability to tailor the base image to specific needs.
   - Users might need to add additional layers or configurations, potentially introducing complexity.

5. **Latency in Updates**:
   - Sometimes, there might be a delay between discovering a vulnerability and releasing a patch, leaving users exposed in the interim.
   - Users need to actively monitor for updates and apply them promptly.
