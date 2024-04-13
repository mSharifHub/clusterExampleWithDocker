

Linkedin Article Link : https://www.linkedin.com/posts/mohamed-sharif-47301520b_learning-by-practice-in-how-to-make-a-database-activity-7184512903914352640-YO3c?utm_source=share&utm_medium=member_desktop
===================================================================================================================================================================
Mohamed Sharif
Seeking Entry Level Software Engineer
2 articles
April 12, 2024

So, here I am with another data related article. I am not going lie that of all my college electives I was mostly in love with computer networks and relational databases. What I like the most and dislike the most are the introduction of very important concepts but without knowing "Hey, hold on, how do I do this ?!". So, I am in a little adventure post graduation in improving my set skills with hope of earning the recognition and be called a software engineer and not a developer. Yet, reality is me and many of use here still spending lot time sending job applications as junior developers. Nonetheless, what I like about software engineering is that all we need is a laptop, internet, a paper and a pencil, and therefore we can be the Bradley Cooper from limetless movie but without the blue pill. So, going back to business, maybe you heard the advantages of nosql and specifically the terms of High Availability and fault tolerance. So, I started asking myself the term sounds simple but in practice how could that be implemented. So, after spending some time and while learning Design Patterns I came to realization of why not just draw a concept 

So, each node can represent a virtual machine or a container. With node a being the entry point node b and node c connects to node a. So, here we have a cluster. Then, we can simply make a cluster in docker.


vim docker-compose.yml


version: "3.7"
services:
  node_a:
    image: cassandra:latest
    container_name: node_a
    environment:
      - CASSANDRA_CLUSTER_NAME=TestCluster
      - CASSANDRA_SEEDS=node_a
    volumes:
      - ./node_a_data:/var/lib/cassandra
    ports:
      - "9042:9042"
  node_b:
    image: cassandra:latest
    container_name: node_b
    environment:
      - CASSANDRA_CLUSTER_NAME=TestCluster
      - CASSANDRA_SEEDS=node_a
    volumes:
      - ./node_b_data:/var/lib/cassandra
    ports:
      - "9043:9042"

  node_c:
    image: cassandra:latest
    container_name: node_c
    environment:
    - CASSANDRA_CLUSTER_NAME=TestCluster
    - CASSANDRA_SEEDS=node_a
    volumes:
    - ./node_c_data:/var/lib/cassandra
    ports:
    - "9044:9042" 


With docker compose we can define and run multiple containers. Here, the services is represented by each container or a microservice. Then we have the image , volume name, container name, and mapping volume to container and local host port to cassandra port. Since we have multiple nodes we want use different ports for each node. The environment are the env variables used in cassandra and we setting the cluster name and node_a as the contact point for any new node joining the cluster. There we can say how we can scale horizontally. I always wanted to know how in practice since it sounds cool the wording. After that we then run 

docker-compose up -d

 it will create the containers and have it ready for us in detach mode


We then connect by

docker exec -it node_a cqlsh

 Now the Keyspace and replication keywords will make sense

CREATE KEYSPACE demo WITH replication = {
    'class': 'SimpleStrategy', 
    'replication_factor': 3
}; 

with this code we can simply create a table and insert data. You can verify that the data was duplicated by exiting and making another exec command but with node name -e flag (execute a command) and a command such as "Select * from database.table_name". 


    The moral of story: 

We retain most of information by trial and errors. I don't think we should use flash cards for computer science or engineering majors. How fun is making a cluster :)


Git repository with the code sample

https://github.com/mSharifHub/clusterExampleWithDocker 
