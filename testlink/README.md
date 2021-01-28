### testlink

以docker-compose方式部署testlink。

```bash
mkdir -p /home/testlink/{mariadb,testlink}/data

docker-compose up -d

docker-compose ps

  Name                Command               State                       Ports                     
--------------------------------------------------------------------------------------------------
mariadb    /opt/bitnami/scripts/maria ...   Up      3306/tcp                                      
testlink   /opt/bitnami/scripts/testl ...   Up      0.0.0.0:8088->8080/tcp, 0.0.0.0:8443->8443/tcp
```

访问 `ip:8088`，账号密码：`testlink`/`testlink`。

---
