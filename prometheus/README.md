### prometheus + grafana

通过docker-compose部署 prometheus + grafana。

```bash
mkdir -p /home/prom/{prometheus/data,grafana}

chmod 777 /home/prom/{prometheus/data,grafana}

docker-compose up -d

docker-compose ps

        Name                      Command               State                       Ports                     
--------------------------------------------------------------------------------------------------------------
prom_alertmanager_1    /bin/alertmanager --config ...   Up      0.0.0.0:9093->9093/tcp, 0.0.0.0:9094->9094/tcp
prom_dingtalk_1        /bin/prometheus-webhook-di ...   Up      0.0.0.0:8060->8060/tcp                        
prom_grafana_1         /run.sh                          Up      0.0.0.0:3000->3000/tcp                        
prom_node-exporter_1   /bin/node_exporter               Up      0.0.0.0:9100->9100/tcp                        
prom_prometheus_1      /bin/prometheus --config.f ...   Up      0.0.0.0:9090->9090/tcp

docker ps

CONTAINER ID        IMAGE                                          COMMAND                  CREATED             STATUS              PORTS                              NAMES
c1ec4cc9c41f        grafana/grafana:latest                         "/run.sh"                36 seconds ago      Up 36 seconds       0.0.0.0:3000->3000/tcp             prom_grafana_1
8cd521c327d8        prom/prometheus:latest                         "/bin/prometheus --c…"   37 seconds ago      Up 36 seconds       0.0.0.0:9090->9090/tcp             prom_prometheus_1
ef93c8c06ca0        prom/alertmanager:latest                       "/bin/alertmanager -…"   37 seconds ago      Up 37 seconds       0.0.0.0:9093-9094->9093-9094/tcp   prom_alertmanager_1
d358a2a39b8d        timonwong/prometheus-webhook-dingtalk:latest   "/bin/prometheus-web…"   38 seconds ago      Up 37 seconds       0.0.0.0:8060->8060/tcp             prom_dingtalk_1
366ff81e7a65        prom/node-exporter:latest                      "/bin/node_exporter"     38 seconds ago      Up 37 seconds       0.0.0.0:9100->9100/tcp             prom_node-exporter_1
```

容器启动正常，访问`ip:9090`，

![Image text](https://github.com/Tobewont/docker-compose/blob/master/prometheus/png/prom-1.png)

访问`ip:3000`，

![Image text](https://github.com/Tobewont/docker-compose/blob/master/prometheus/png/prom-2.png)

可以看到，prometheus各组件状态正常。

---

### node-exporter

其实node-exporter不需要通过docker-compose启动，对于每个要监控的主机，直接docker启动node-exporter：

```bash
docker pull prom/node-exporter:latest

docker run -d -p 9100:9100 --name node-exporter prom/node-exporter:latest
```

之后修改`prometheus.yml`，重启所有容器：

```bash
docker-compose restart
```

---

### 测试告警

```bash
docker stop prom_node-exporter_1

docker-compose ps

        Name                      Command               State                        Ports                     
---------------------------------------------------------------------------------------------------------------
prom_alertmanager_1    /bin/alertmanager --config ...   Up       0.0.0.0:9093->9093/tcp, 0.0.0.0:9094->9094/tcp
prom_dingtalk_1        /bin/prometheus-webhook-di ...   Up       0.0.0.0:8060->8060/tcp                        
prom_grafana_1         /run.sh                          Up       0.0.0.0:3000->3000/tcp                        
prom_node-exporter_1   /bin/node_exporter               Exit 2                                                 
prom_prometheus_1      /bin/prometheus --config.f ...   Up       0.0.0.0:9090->9090/tcp
```

![Image text](https://github.com/Tobewont/docker-compose/blob/master/prometheus/png/prom-3.png)

收到钉钉和邮件故障告警，

![Image text](https://github.com/Tobewont/docker-compose/blob/master/prometheus/png/prom-4.png)

![Image text](https://github.com/Tobewont/docker-compose/blob/master/prometheus/png/prom-5.png)

```bash
docker start prom_node-exporter_1

docker-compose ps

        Name                      Command               State                       Ports                     
--------------------------------------------------------------------------------------------------------------
prom_alertmanager_1    /bin/alertmanager --config ...   Up      0.0.0.0:9093->9093/tcp, 0.0.0.0:9094->9094/tcp
prom_dingtalk_1        /bin/prometheus-webhook-di ...   Up      0.0.0.0:8060->8060/tcp                        
prom_grafana_1         /run.sh                          Up      0.0.0.0:3000->3000/tcp                        
prom_node-exporter_1   /bin/node_exporter               Up      0.0.0.0:9100->9100/tcp                        
prom_prometheus_1      /bin/prometheus --config.f ...   Up      0.0.0.0:9090->9090/tcp
```

收到钉钉和邮件恢复告警，

![Image text](https://github.com/Tobewont/docker-compose/blob/master/prometheus/png/prom-6.png)

![Image text](https://github.com/Tobewont/docker-compose/blob/master/prometheus/png/prom-7.png)

测试宕机完成，告警没有问题。

docker-compose部署 prometheus + grafana 完成。

---
