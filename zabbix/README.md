### zabbix

以docker-compose方式部署zabbix。

```bash
mkdir -p /data/mysql

docker-compose up -d

docker-compose ps

         Name                       Command               State                 Ports              
---------------------------------------------------------------------------------------------------
mysql                    docker-entrypoint.sh --cha ...   Up      0.0.0.0:3306->3306/tcp, 33060/tcp
zabbix-agent             /sbin/tini -- /usr/bin/doc ...   Up      0.0.0.0:10050->10050/tcp         
zabbix-server            /sbin/tini -- /usr/bin/doc ...   Up      0.0.0.0:10051->10051/tcp         
zabbix-web-nginx-mysql   docker-entrypoint.sh             Up      0.0.0.0:8080->8080/tcp, 8443/tcp
```

访问`ip:8080`，账号/密码：`Admin`/`zabbix`。

![Image text](https://github.com/Tobewont/docker-compose/blob/master/zabbix/png/zabbix-1.png)

查看 zabbix agent 日志报错：`no active checks on server [192.168.30.135:10051]: host [192.168.30.135] not found`

解决：zabbix web 界面添加host，hostname 为 192.168.30.135。

![Image text](https://github.com/Tobewont/docker-compose/blob/master/zabbix/png/zabbix-2.png)

通过 docker-compose 部署 zabbix 完成。

---
