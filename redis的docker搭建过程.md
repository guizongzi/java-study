## redis

Redis是一个开源（BSD许可），内存存储的数据结构服务器，可用作数据库，高速缓存和消息队列代理。它支持[字符串](https://www.redis.net.cn/tutorial/3508.html)、[哈希表](https://www.redis.net.cn/tutorial/3509.html)、[列表](https://www.redis.net.cn/tutorial/3510.html)、[集合](https://www.redis.net.cn/tutorial/3511.html)、[有序集合](https://www.redis.net.cn/tutorial/3512.html)，[位图](https://www.redis.net.cn/tutorial/3508.html)，[hyperloglogs](https://www.redis.net.cn/tutorial/3513.html)等数据类型。内置复制、[Lua脚本](https://www.redis.net.cn/tutorial/3516.html)、LRU收回、[事务](https://www.redis.net.cn/tutorial/3515.html)以及不同级别磁盘持久化功能，同时通过Redis Sentinel提供高可用，通过Redis Cluster提供自动[分区](https://www.redis.net.cn/tutorial/3524.html)。

### (一)***\*自定义docker静态网段\****

```bash
docker network create --driver bridge --subnet=172.18.12.0/16 --gateway=172.18.1.1 wn_docker_net
docker network ls
```

### (二)***\*拉取docker\****

```bash
docker pull redis
```

### (三)创建容器

1. **创建文件夹**

   ```bash
   [root@localhost software]#
   mkdir -p redis/6379/conf redis/6379/data redis/6379/log
   ```

2. **将redis.conf上传到conf**

   [redis.conf](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/46c3c19e-4261-443c-ad73-5991fe1ee427/redis.conf)

3. **创建日志文件**

   ```bash
   [root@localhost log]# 
   touch redis.log
   ```

4. **开放日志文件读写权限**

   ```bash
   [root@localhost log]# 
   chmod 777 redis.log
   ```

5. **修改配置文件**

   ```bash
   [root@localhost conf]# 
   vim redis.conf
   
   75 bind 0.0.0.0 #ip
   94 protected-mode no #关闭保护模式
   304 logfile "/var/log/redis.log" #容器内的日志位置
   ```

6. **创建并启动容器**

   ```bash
   docker run -it \\
   --name redis_6379 \\
   --privileged \\
   -p 6379:6379 \\
   --network wn_docker_net \\
   --ip 172.18.12.10 \\
   -v /usr/local/software/redis/6379/conf/redis.conf:/usr/local/etc/redis/redis.conf \\
   -v /usr/local/software/redis/6379/data/:/data \\
   -v /usr/local/software/redis/6379/log/redis.log:/var/log/redis.log \\
   -d redis \\
   /usr/local/etc/redis/redis.conf
   ```

7. **开放防火墙**

   ```bash
   [root@localhost log]# firewall-cmd --zone=public --add-port=6379/tcp --permanent
   success
   [root@localhost log]# firewall-cmd --reload
   success
   ```

### (三)测试redis

1. **检查进程是否启动**

   ```bash
   [root@localhost log]# 
   docker ps
   ```

2. **查看日志**

   ```bash
   [root@localhost log]# 
   docker logs redis_6379
   [root@localhost log]# 
   docat redis.log
   ```

3. ***\*测试redis\****

   ```bash
   [root@localhost log]# docker exec -it redis_6379 bash
   root@42a18ce67e14:/data# redis-cli
   127.0.0.1:6379> ping
   PONG
   127.0.0.1:6379> exit
   root@42a18ce67e14:/data# exit
   exit
   ```