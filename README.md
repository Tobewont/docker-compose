# elfk-docker

以docker-compose方式部署elk

```bash
docker-compose up --build -d
```

### filebeat

对于filebeat，分布式部署。以收集 `/home/logs/` 目录下所有日志为例，映射目录时 `rw` 权限不可少。

```bash
cd filebeat/

docker build -t elfk_filebeat:latest .

docker run -d \
  --name=filebeat \
  --user=root \
  -v /var/lib/docker/containers:/var/lib/docker/containers:ro \
  -v /var/run/docker.sock:/var/run/docker.sock:ro \
  -v /home/logs/:/home/logs/:rw \
  elfk_filebeat:latest filebeat -e -strict.perms=false \
  -E output.elasticsearch.hosts=["localhost:9200"]
```
