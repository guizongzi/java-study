## redis主从

### 创建从redis容器

1. **创建文件夹**

2. **创建日志文件并开放权限**

3. **开放防火墙**

4. **将主redis配置文件复制到从6380/conf下，并修改**

   ```bash
   [root@localhost conf]# 
   vim redis.conf
   
   479 replicaof 172.18.12.10 6379
   480 slave-read-only no
   1255 appendonly yes
   ```

5. **创建并启动容器**

   ```bash
   docker run -it \\
   --name redis_6380 \\
   --privileged \\
   -p 6380:6379 \\
   --network wn_docker_net \\
   --ip 172.18.12.11 \\
   -v /usr/local/software/redis/6380/conf/redis.conf:/usr/local/etc/redis/redis.conf \\
   -v /usr/local/software/redis/6380/data/:/data \\
   -v /usr/local/software/redis/6380/log/redis.log:/var/log/redis.log \\
   -d redis \\
   /usr/local/etc/redis/redis.conf
   ```

### 测试

1. **查看从redis容器**

   ```bash
   [root@localhost ~]# docker exec -it redis_6380 bash
   root@773b15e50a4b:/data# redis-cli
   127.0.0.1:6379> info replication
   # Replication
   role:slave #salve
   master_host:172.18.12.10 #master地址
   ...
   ```

2. **查看主redis容器**

   ```bash
   [root@localhost ~]# docker exec -it redis_6379 bash
   root@42a18ce67e14:/data# redis-cli
   127.0.0.1:6379> info replication
   # Replication
   role:master #master
   connected_slaves:2 #有2个从与其相连
   ```