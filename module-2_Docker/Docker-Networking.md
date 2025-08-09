# Day-10 Docker Networking with Hands on Demo

### 🔹 What is Docker Networking?
Docker provides network drivers that allow containers to communicate with:
 - Each other (inter-container)
 - The host machine
 - The internet

### 🧠 Docker Network Types
| Network Type       | Description                                     |
| ------------------ | ----------------------------------------------- |
| `bridge` (default) | Used for standalone containers on a single host |
| `host`             | Shares the host network stack (no isolation)    |
| `none`             | No networking                                   |

We’ll focus on bridge networking here, as it’s most commonly used.

### 🛠 Hands-On Docker Networking Demo

First create 2 container 
```sh
docker container run -itd --name=a1 centos:7
docker container run -itd --name=a2 centos:7
```
Now try to ping from one container to another
```sh
docker exec a1 ping a2
```
We see not able to get the ping reply. <br>
Now create a network then create 2 contaier using this net work
First craete a network
```sh
docker network create --driver=bridge --subnet=192.168.10.0/24 net1
```
```sh
docker container run -itd --name=b1 --net=net1 centos:7
docker container run -itd --name=b1 --net=net1 centos:7
```
Now try ping
```sh
docker exec b1 ping b2
```
Now we can see from b1 to b2 we can get ping. <br>
Now add the previous a1 container to this network
```sh
 docker network connect net1 a1
```
Now try ping form b1 to a1
```sh
docker exec b1 ping a1
```
Now we get the ping
Now disconnect from a1 from the network (net1)
```sh
docker network disconnect net1 a1
```
Now we can not get the ping from b1 to a1
Now delete the network, but before delete the network make sure no conatiner use this network, if use then first delete those conatiner then delete the network
```sh
docker container rm $(docker container ls -aq)                # Stop and Delete all container
docker network rm net1                                        # delete the network
```

## 🌐 Networks Command list
```sh
docker network ls                           # List Docker networks
docker network create <network-name>        # Create network
docker network inspect <name>               # Inspect network
docker network connect <net> <container>    # Connect container to network
docker network rm <name>                    # Remove network
```
