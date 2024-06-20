# Setting Up a 6-Node Redis Cluster

This guide provides step-by-step instructions for setting up a Redis cluster with 6 nodes (3 masters and 3 slaves).

## Configuration Overview

Each of the 6 nodes will have its own configuration file. The nodes are organized in the following manner:
- **7001**: Master 1
- **7002**: Master 2
- **7003**: Master 3
- **7004**: Slave of Master 1
- **7005**: Slave of Master 2
- **7006**: Slave of Master 3

Ensure each node has a unique configuration file named `nX_redis.conf` (where X is the node number).

## 1. Start Redis Servers

### Node 7001

Navigate to the folder for node 7001 and run the Redis server:
```bash
redis-server n1_redis.conf &
```

### Node 7002

Navigate to the folder for node 7002 and run the Redis server:
```bash
redis-server n2_redis.conf &
```

### Node 7003

Navigate to the folder for node 7003 and run the Redis server:
```bash
redis-server n3_redis.conf &
```

### Node 7004

Navigate to the folder for node 7004 and run the Redis server:
```bash
redis-server n4_redis.conf &
```

### Node 7005

Navigate to the folder for node 7005 and run the Redis server:
```bash
redis-server n5_redis.conf &
```

### Node 7006

Navigate to the folder for node 7006 and run the Redis server:
```bash
redis-server n6_redis.conf &
```

## 2. Check Running Nodes

To verify how many nodes are running in the cluster, use:
```bash
ps -ef | grep redis
```

## 3. Check Node Roles and Cluster Information

### 3.1. Login to a Redis Node

Login to any running slave/master node using `redis-cli`:
```bash
redis-cli -c -p 7005
```

### 3.2. Get Master-Slave Information

Get the information of all nodes in the cluster:
```bash
cluster nodes
```

### 3.3. Monitor Node Elections and Failover

To monitor elections and the failover process when a master node goes down:
1. Open the Redis CLI for each node.
2. Shut down any master node and observe its corresponding slave node.

To shut down a master node:
```bash
redis-cli -c -p <master_port> shutdown
```

Monitor the corresponding slave node for logs and failover events.

## 4. Shutdown Nodes

### 4.1. Shutdown Slaves First

To avoid unnecessary logs (e.g., slaves promoting to masters), shut down slaves first, followed by masters.

Example to shut down node 7001:
```bash
redis-cli -p 7001 shutdown
```

Repeat the shutdown process for all slave nodes before shutting down master nodes.

```bash
redis-cli -p 7004 shutdown
redis-cli -p 7005 shutdown
redis-cli -p 7006 shutdown
```

### 4.2. Shutdown Masters

After shutting down the slaves, proceed to shut down the master nodes.

```bash
redis-cli -p 7001 shutdown
redis-cli -p 7002 shutdown
redis-cli -p 7003 shutdown
```
