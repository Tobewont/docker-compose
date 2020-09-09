### apollo

以docker-compose方式部署apollo。

- 数据库准备：

首先部署mysql，创建用户并设置密码，这里是`root`/`123456789`。

```bash
cd /software

git clone https://github.com/ctripcorp/apollo.git

mysql -uroot -p123456789 < apollo/scripts/sql/apolloportaldb.sql

mysql -uroot -p123456789 < apollo/scripts/sql/apolloconfigdb.sql
```

- 部署：

参数说明：

```a
SPRING_DATASOURCE_URL: 对应环境ApolloPortalDB的地址

SPRING_DATASOURCE_USERNAME: 对应环境ApolloPortalDB的用户名

SPRING_DATASOURCE_PASSWORD: 对应环境ApolloPortalDB的密码

APOLLO_PORTAL_ENVS(可选): 对应ApolloPortalDB中的apollo.portal.envs配置项，如果没有在数据库中配置的话，可以通过此环境参数配置

DEV_META/PRO_META(可选): 配置对应环境的Meta Service地址，以${ENV}_META命名，如果ApolloPortalDB中配置了apollo.portal.meta.servers，则以apollo.portal.meta.servers中的配置为准
```

```bash
docker-compose up -d

docker-compose ps

        Name                      Command               State           Ports         
--------------------------------------------------------------------------------------
apollo-adminservice    /apollo-adminservice/scrip ...   Up      0.0.0.0:8090->8090/tcp
apollo-configservice   /apollo-configservice/scri ...   Up      0.0.0.0:8080->8080/tcp
apollo-portal          /apollo-portal/scripts/sta ...   Up      0.0.0.0:8070->8070/tcp
```

- 访问ui：

访问`ip:8070`，账号/密码：`apollo`/`admin`。

---
