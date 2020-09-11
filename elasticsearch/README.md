### elasticsearch

通过docker-compose部署es集群。

```bash
mkdir -p /home/elfk/elasticsearch/{data1,data2,data3}

if [ $(grep 'vm.max_map_count' /etc/sysctl.conf |wc -l) -eq 0 ] ; \
then echo 'vm.max_map_count=655360' >> /etc/sysctl.conf; \
fi

sysctl -p

docker-compose up --build -d
```

```bash
docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                              NAMES
5af70e32dbb8        elfk_es01           "/usr/local/bin/dock…"   14 seconds ago      Up 6 seconds        0.0.0.0:9200->9200/tcp, 9300/tcp   es01
793bab4160b7        elfk_es03           "/usr/local/bin/dock…"   14 seconds ago      Up 6 seconds        9200/tcp, 9300/tcp                 es03
93ffa61c639f        elfk_es02           "/usr/local/bin/dock…"   14 seconds ago      Up 6 seconds        9200/tcp, 9300/tcp                 es02

netstat -lntp

Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      1303/master         
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      936/sshd            
tcp6       0      0 ::1:25                  :::*                    LISTEN      1303/master         
tcp6       0      0 :::9200                 :::*                    LISTEN      8724/docker-proxy   
tcp6       0      0 :::22                   :::*                    LISTEN      936/sshd
```

通过docker-compose部署es集群完成。

---

`bootstrap.memory_lock: true`报错：`memory locking requested for elasticsearch process but memory is not locked`


解决：

```bash
vim /etc/security/limits.conf

* soft memlock unlimited
* hard memlock unlimited

if [ $(grep 'vm.swappiness=0' /etc/sysctl.conf |wc -l) -eq 0 ] ; \
then echo 'vm.swappiness=0' >> /etc/sysctl.conf; \
fi

sysctl -p
```

---
