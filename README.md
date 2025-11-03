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

![instance](https://github.com/user-attachments/assets/51b9ee10-8681-4245-985d-15c99897aaa4)

Lets install docker on each node

yum install docker -y && systemctl start docker                                                                                                                         

# Step 1: Initialize the Swarm on Master

docker swarm init  

<img width="2880" height="884" alt="docker swarm init" src="https://github.com/user-attachments/assets/dfe65721-45fc-436b-9772-4da90fdbf11a" />

This command initializes Swarm mode and generates a token. If we copy the token and paste it on worker node, then the worker node will join in the docker swarm cluster.

## worker-1:

<img width="2870" height="110" alt="worker-1" src="https://github.com/user-attachments/assets/7d3014e6-600e-412b-88c2-a00e282ef4b9" />

## worker-2:

<img width="2828" height="174" alt="worker-2" src="https://github.com/user-attachments/assets/507f242c-cd82-4c9c-ba9b-5cca00f00563" />

# Step 2: Verify the Cluster

On the manager node, check the status of the cluster:


