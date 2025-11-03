# Scalable-Web-Service-Deployment-Using-Docker-Swarm


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

![instance](https://github.com/user-attachments/assets/51b9ee10-8681-4245-985d-15c99897aaa4)

Lets install docker on each node

yum install docker -y && systemctl start docker                                                                                                                         

## Step 1: Initialize the Swarm on Master

docker swarm init  

<img width="2880" height="884" alt="docker swarm init" src="https://github.com/user-attachments/assets/dfe65721-45fc-436b-9772-4da90fdbf11a" />

This command initializes Swarm mode and generates a token. If we copy the token and paste it on worker node, then the worker node will join in the docker swarm cluster.

## worker-1:

<img width="2870" height="110" alt="worker-1" src="https://github.com/user-attachments/assets/7d3014e6-600e-412b-88c2-a00e282ef4b9" />

## worker-2:

<img width="2828" height="174" alt="worker-2" src="https://github.com/user-attachments/assets/507f242c-cd82-4c9c-ba9b-5cca00f00563" />

## Step 2: Verify the Cluster

On the manager node, check the status of the cluster:

docker node ls

You should see a list of all nodes in the cluster.

<img width="2708" height="214" alt="node-cluster" src="https://github.com/user-attachments/assets/416db6db-3dbb-429b-aa76-04e8c7adcf3c" />

## Deploying a Service in Docker Swarm

Let’s deploy a simple application across the cluster.

docker service create --name bhargav --replicas 3 -p 8081:80 bhargav/dm

<img width="2880" height="498" alt="docker-srvc" src="https://github.com/user-attachments/assets/c2d90f17-d880-47c4-96b1-6b4fae4cfc90" />

## This command:

Creates a service named bhargav

Runs 3 replicas (instances) of the mustafa container

Exposes port 8081 on all nodes

## OUTPUT FROM MANAGER NODE:

![manager-node](https://github.com/user-attachments/assets/5421a182-5b06-4d67-ab17-f746ae1d0551)


## OUTPUT FROM WORKER-1 NODE:

![workernode-1](https://github.com/user-attachments/assets/18fc2d9b-9a40-4da2-b436-f1958fb6d63e)


## OUPUT FROM WORKER-2 NODE:

![WORKERNODE-2](https://github.com/user-attachments/assets/3a1f4fef-9c5b-4391-b8da-674a5f4d0d23)



## Checking Service Status

To check if the service is running, use:

docker service ls

<img width="2572" height="248" alt="status" src="https://github.com/user-attachments/assets/8acf97d9-d07c-4b55-99f4-b40e68adac87" />

To see details of running tasks (containers), use:

docker service ps mustafa

<img width="2878" height="238" alt="ps-mstfa" src="https://github.com/user-attachments/assets/e73159cc-d733-4413-bf2f-2548ee060726" />

## Scaling Services in Docker Swarm

Scaling is effortless in Swarm. If traffic increases, you can scale up with:

docker service scale mustafa=5

This increases the Nginx service replicas to 5. Swarm automatically distributes them across available nodes.

![avlable-nodes](https://github.com/user-attachments/assets/9ee61f80-5f91-42b2-b228-ba25d81aee33)

Now lets check the number of containers, previously we have 3 containers now we scale up to 5, it will show 5 containers for your services

docker service ps mustafa

## Rolling Updates in Docker Swarm

Imagine you need to update your application to a newer version. Instead of taking everything down, Swarm allows rolling updates:

sudo docker service update --image shaikmustafa/cycle mustafa

Swarm updates containers one at a time, ensuring zero downtime.

![updatd-containers](https://github.com/user-attachments/assets/27d1de88-863d-4d58-9075-807e08f6b942)

## OUTPUT FROM MANAGER NODE:

![OUTPUT-MAIN](https://github.com/user-attachments/assets/b6e6aa56-ac4f-436b-84f3-32194ad8f9da)

## OUPUT FROM WORKER-1 NODE:

![OUTPUT-MAIN](https://github.com/user-attachments/assets/e8579e26-1fe2-4676-8b47-7d759efccf0c)

## OUPUT FROM WORKER-12 NODE:

![OUTPUT-MAIN](https://github.com/user-attachments/assets/03c37d13-f207-44ef-b207-df5758c92ce1)

## Rollback in Docker Swarm

docker service rollback bhargav

![rollback](https://github.com/user-attachments/assets/bc0323b8-7aed-4eed-ab7b-3377679443d5)

Now check the output, you can see the old output in browser

<img width="2880" height="1660" alt="b104c7e9-c260-4347-82bc-888007c72bd3" src="https://github.com/user-attachments/assets/a0c1b0ed-6670-4aee-97f0-86d7485388b6" />

## OUPUT FROM WORKER-1 NODE:

<img width="2880" height="1660" alt="b104c7e9-c260-4347-82bc-888007c72bd3" src="https://github.com/user-attachments/assets/e4af34f6-246a-44bf-924a-070d7b32e51e" />

## OUPUT FROM WORKER-2 NODE:

<img width="2880" height="1660" alt="b104c7e9-c260-4347-82bc-888007c72bd3" src="https://github.com/user-attachments/assets/64bdcbb0-f204-49c4-8d9d-483d9c0aa0c0" />

## Remove a Service in Docker Swarm

docker service rm mustafa

When we remove a service, it will delete the containers also

![rm-mstfa](https://github.com/user-attachments/assets/62aacede-756b-4ec2-8da2-4e3a6493eb1e)

## Cluster Maintenance in Docker Swarm

Till now we maintained applications on Docker swarm, now let me know you how to maintain cluster in docker swarm. As we know cluster is nothing but a group of servers. It has master node and worker nodes. In the above example we have 3 nodes (1 master & 2 worker nodes). If you want to add or delete a node from cluster here are the commands to follow.

1.To remove a node from cluster, go to that worker node and perform the below command.

 docker swarm leave

![dkr-swm-lve](https://github.com/user-attachments/assets/6ea1bd24-9074-4e3a-b386-52c5a1eeb04f)

After 10 seconds, this node went to Down state in cluster.

 docker node ls

<img width="2880" height="422" alt="image" src="https://github.com/user-attachments/assets/0bd9c84e-1e63-4ec8-9939-7ac4e68942bd" />

Now remove the node from the cluster

 docker node rm <node-id>

<img width="1694" height="186" alt="image" src="https://github.com/user-attachments/assets/083bea45-97b4-4b86-a07e-f8c91fb63c20" />

Now see we have have only 2 Nodes

<img width="2880" height="328" alt="image" src="https://github.com/user-attachments/assets/d7a6864e-f0f9-4e47-8024-31c1d65dd8af" />

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


































