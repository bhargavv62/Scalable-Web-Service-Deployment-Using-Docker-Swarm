## Description

This project demonstrates the deployment of a scalable web service using Docker Swarm for container orchestration. As a DevOps Engineer, I utilized Docker Swarm to efficiently manage containerized applications across multiple nodes, ensuring reliability, scalability, and simplified management.

While Kubernetes is often the preferred orchestration platform, Docker Swarm provides a lightweight and straightforward alternative, making it ideal for smaller-scale environments or teams seeking easier adoption.

## What is Docker Swarm?
  Docker Swarm is a native clustering and orchestration tool for Docker containers. It enables you to manage a group of Docker nodes as a single virtual system, simplifying container deployments and scaling. Swarm mode is built into Docker, making it an attractive choice for teams already using Docker. We have to deploy the application on cluster.

## What is Cluster?
In Docker Swarm, a cluster is a group of multiple Docker nodes that work together as a single system to run containerized applications.

A Swarm cluster consists of:

• Manager Nodes – Control and manage the cluster and services.

• Worker Nodes – Run the application containers.

The manager node assigns tasks to worker nodes, ensuring high availability and scalability. This allows applications to run smoothly even if some nodes fail.

## How to create a cluster in Docker Swarm?

Let’s walk through a real-world scenario where we deploy a multi-node Docker Swarm cluster.

Launch 3 EC2 Instances (1 Master, 2 Worker Nodes).

<img width="1520" height="367" alt="instance" src="https://github.com/user-attachments/assets/7f7f8716-08cc-41e0-bc5c-95a6ff62a41c" />


Lets install docker on each node

    yum install docker -y && systemctl start docker                                                                                                                         

## Step 1: Initialize the Swarm on Master

    docker swarm init  

<img width="1591" height="287" alt="swarm-init" src="https://github.com/user-attachments/assets/38d6c89c-f2fa-47e8-af31-cd55204dc62d" />


This command initializes Swarm mode and generates a token. If we copy the token and paste it on worker node, then the worker node will join in the docker swarm cluster.

## worker-1:

<img width="1599" height="236" alt="worker-1" src="https://github.com/user-attachments/assets/14b894fa-ac8a-4d1f-a710-555c09e86635" />


## worker-2:

<img width="1595" height="157" alt="worker-2" src="https://github.com/user-attachments/assets/6cc5fb28-4d03-4826-8d9c-d4d77af8a819" />


## Step 2: Verify the Cluster

On the manager node, check the status of the cluster:

    docker node ls

You should see a list of all nodes in the cluster.

<img width="1592" height="176" alt="node-ls" src="https://github.com/user-attachments/assets/fb9c88fb-27de-414e-8445-b90a7c3c3c1e" />


## Deploying a Service in Docker Swarm

Let’s deploy a simple application across the cluster.

    docker service create --name bhargav --replicas 3 -p 8081:80 bhargav62/dm

<img width="1595" height="218" alt="bhargav62dm" src="https://github.com/user-attachments/assets/3a4e40c2-6497-4714-b6e2-426edc7f3012" />


## This command:

Creates a service named bhargav

Runs 3 replicas (instances) of the bhargav container

Exposes port 8081 on all nodes

## OUTPUT FROM MANAGER NODE:

<img width="1920" height="943" alt="manager-1" src="https://github.com/user-attachments/assets/fd30b5cb-673f-4148-bd09-9e5aeebae064" />



## OUTPUT FROM WORKER-1 NODE:

<img width="1920" height="936" alt="worker-1-0t" src="https://github.com/user-attachments/assets/aa32f3d6-6f91-4ad3-8a08-d9dcac0ba9f1" />


## OUTPUT FROM WORKER-2 NODE:

<img width="1920" height="941" alt="worker-2-ot" src="https://github.com/user-attachments/assets/f9fd0a98-7859-43c9-a65b-e23be6dbd341" />




## Checking Service Status

To check if the service is running, use:

    docker service ls

<img width="1085" height="113" alt="service-ls" src="https://github.com/user-attachments/assets/1d6ac734-bd5c-448b-98e8-e170fb3b25d7" />


To see details of running tasks (containers), use:

    docker service ps bhargav

<img width="1551" height="214" alt="service-ps" src="https://github.com/user-attachments/assets/19eee72b-22e6-49c2-974d-c0d2ca2bfca9" />


## Scaling Services in Docker Swarm

Scaling is effortless in Swarm. If traffic increases, you can scale up with:

    docker service scale bhargav=5

This increases the Nginx service replicas to 5. Swarm automatically distributes them across available nodes.

<img width="1209" height="258" alt="scale=5" src="https://github.com/user-attachments/assets/26addde7-b24f-43a3-afa1-b4790e77083b" />


Now lets check the number of containers, previously we have 3 containers now we scale up to 5, it will show 5 containers for your services

    docker service ps bhargav

    

## Rolling Updates in Docker Swarm

Imagine you need to update your application to a newer version. Instead of taking everything down, Swarm allows rolling updates:

    sudo docker service update --image bhargav62/cycle bhargav

Swarm updates containers one at a time, ensuring zero downtime.

<img width="1269" height="307" alt="service-up-cycle" src="https://github.com/user-attachments/assets/b706ce1f-ccef-493d-9b9b-befb2bc2b796" />


## OUTPUT FROM MANAGER NODE:

<img width="1920" height="935" alt="master-op" src="https://github.com/user-attachments/assets/f5ab99bb-b4be-4255-9ab0-d5e368d3f715" />


## OUTPUT FROM WORKER-1 NODE:

<img width="1920" height="930" alt="worker-1-op" src="https://github.com/user-attachments/assets/f17455ec-3609-4b4c-9e4c-439d65e8917b" />



## OUTPUT FROM WORKER-2 NODE:

![WKND-2](https://github.com/user-attachments/assets/1473dd21-31d7-46a6-a504-f8b4193d9c7d)


## Rollback in Docker Swarm

    docker service rollback bhargav

<img width="982" height="328" alt="service-rollback" src="https://github.com/user-attachments/assets/c5064861-d1a7-4cac-a60b-703bd39fcc8d" />



Now check the output, you can see the old output in browser

## OUTPUT FROM MANAGER NODE:

<img width="2880" height="1660" alt="MNG" src="https://github.com/user-attachments/assets/121d466d-1bf2-45de-9b6e-ee31be9f722c" />


## OUTPUT FROM WORKER-1 NODE:

![ND-1](https://github.com/user-attachments/assets/0efa70cc-02fc-4ed9-95ae-4b64b9830da5)


## OUTPUT FROM WORKER-2 NODE:

<img width="1920" height="945" alt="worker-2-op" src="https://github.com/user-attachments/assets/446464b6-2b00-4fb5-b3c2-b08f4f456705" />



## Remove a Service in Docker Swarm

    docker service rm bhargav

When we remove a service, it will delete the containers also

<img width="852" height="57" alt="service-rm" src="https://github.com/user-attachments/assets/2f3a48ea-5a71-4114-8f8d-5569ba1b32a4" />


## Cluster Maintenance in Docker Swarm

Till now we maintained applications on Docker swarm, now let me know you how to maintain cluster in docker swarm. As we know cluster is nothing but a group of servers. It has master node and worker nodes. In the above example we have 3 nodes (1 master & 2 worker nodes). If you want to add or delete a node from cluster here are the commands to follow.

1.To remove a node from cluster, go to that worker node and perform the below command.

    docker swarm leave

<img width="750" height="98" alt="node1-leave" src="https://github.com/user-attachments/assets/485ecbed-f8b9-4bf9-aed0-f5897cdff558" />


After 10 seconds, this node went to Down state in cluster.

    docker node ls

<img width="1375" height="182" alt="node-ls-leave" src="https://github.com/user-attachments/assets/655f6c65-35e5-4c60-8ebc-67366d8f8eec" />


Now remove the node from the cluster

    docker node rm <node-id>

<img width="1109" height="129" alt="node-rm" src="https://github.com/user-attachments/assets/3b0d7158-6937-4c9a-bb1f-e57542ee31ad" />


Now see we have have only 2 Nodes

<img width="1376" height="145" alt="node-ls-after-rm" src="https://github.com/user-attachments/assets/75d45d7a-0049-4479-8e82-f06e004284f7" />


2. If you want to add a worker node to our cluster, we need o get a worker node token

       docker swarm join-token worker
 
Now copy the token and paste it on any docker host. It will join as a worker node in your cluster

3. If you want to add a manager node to our cluster, we need o get a worker node token

       docker swarm join-token mnager

Now copy the token and paste it on any docker host. It will join as a manager node in your cluster

# Conclusion

Docker Swarm is a powerful yet simple tool for container orchestration. If you need a lightweight alternative to Kubernetes for managing containers, Swarm is a great choice.

If you're just starting, I recommend setting up a small cluster and experimenting with deployments. Understanding Docker Swarm will give you a solid foundation in container orchestration and help you scale applications effortlessly.


# 👨‍💻 Author & Contributors

Bhargav 💡 maintains this project.Open to feedback, ideas, and contributions are warmly welcomed.

# 🤝 Let’s Collaborate:

GitHub: https://github.com/bhargavv62


# 🚀 Support & Contribute
Found this project useful?

•	Add a ⭐ to show your appreciation

•	Spread the word by sharing it

•	Contributing ideas, fixes, or improvements


































