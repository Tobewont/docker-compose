### yapi

以docker-compose方式部署yapi。

```bash
mkdir -p /data/yapi/{mongo,yapi}

docker-compose up --build
```

打开`ip:9090`，输入相应的配置和点击开始部署，完成整个网站的部署。

![Image text](https://github.com/Tobewont/docker-compose/tree/master/yapi/png/yapi-1.png)

![Image text](https://github.com/Tobewont/docker-compose/tree/master/yapi/png/yapi-2.png)

使用`Ctrl + C`退出，重新修改 `docker-compose.yaml`

```yaml
version: '3.7'

services:
  mongo:
    container_name: mongo
    image: mongo:4
    ports:
      - "27017:27017"
    volumes:
      - type: volume
        source: mongo
        target: /data/db
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example
      MONGO_INITDB_DATABASE: yapi
    restart: always
    volumes:
      - type: bind
        source: ./init-mongo.js
        target: /docker-entrypoint-initdb.d/init-mongo.js
        read_only: true
      - type: volume
        source: mongo
        target: /data/db
    networks:
      - yapi

  yapi:
    depends_on:
      - mongo
    build:
      context: ./
    container_name: yapi
    image: yapi
    #command: "yapi server"              #第一次启动使用
    command: "node /yapi/vendors/server/app.js"             #后面启动使用
    ports: 
      - "9090:9090"
      - "3000:3000"
    restart: always
    volumes:
      - type: volume
        source: yapi
        target: /yapi
    networks:
      - yapi
      
volumes:
  mongo:
    driver: local
    driver_opts:
      type: none
      o: bind
      device: /data/yapi/mongo
  yapi:
    driver: local
    driver_opts:
      type: none
      o: bind
      device: /data/yapi/yapi

networks:
  yapi:
    driver: bridge
```

```bash
docker-compose up -d

docker-compose ps

Name               Command               State                       Ports                     
-----------------------------------------------------------------------------------------------
mongo   docker-entrypoint.sh mongod      Up      0.0.0.0:27017->27017/tcp                      
yapi    docker-entrypoint.sh node  ...   Up      0.0.0.0:3000->3000/tcp, 0.0.0.0:9090->9090/tcp
```

打开`ip:3000`，账号/密码：`admin@admin.com`/`ymfe.org`。

![Image text](https://github.com/Tobewont/docker-compose/tree/master/yapi/png/yapi-3.png)

![Image text](https://github.com/Tobewont/docker-compose/tree/master/yapi/png/yapi-4.png)

至此，以docker-compose方式部署yapi完成。

---
