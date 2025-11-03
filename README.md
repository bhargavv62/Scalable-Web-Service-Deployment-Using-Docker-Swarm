# Scalable-Web-Service-Deployment-Using-Docker-Swarm


## Description

This project demonstrates the deployment of a scalable web service using Docker Swarm for container orchestration. As a DevOps Engineer, I utilized Docker Swarm to efficiently manage containerized applications across multiple nodes, ensuring reliability, scalability, and simplified management.

While Kubernetes is often the preferred orchestration platform, Docker Swarm provides a lightweight and straightforward alternative, making it ideal for smaller-scale environments or teams seeking easier adoption.

# What is Docker Swarm?
  Docker Swarm is a native clustering and orchestration tool for Docker containers. It enables you to manage a group of Docker nodes as a single virtual system, simplifying container deployments and scaling. Swarm mode is built into Docker, making it an attractive choice for teams already using Docker. We have to deploy the application on cluster.

# What is Cluster?
In Docker Swarm, a cluster is a group of multiple Docker nodes that work together as a single system to run containerized applications.

A Swarm cluster consists of:
• Manager Nodes – Control and manage the cluster and services.

• Worker Nodes – Run the application containers.

The manager node assigns tasks to worker nodes, ensuring high availability and scalability. This allows applications to run smoothly even if some nodes fail.

# How to create a cluster in Docker Swarm?

Let’s walk through a real-world scenario where we deploy a multi-node Docker Swarm cluster.

Launch 3 EC2 Instances (1 Master, 2 Worker Nodes).
