### redmine

以docker-compose方式部署redmine。

```bash
mkdir -p /home/bitnami/{mariadb,redmine}

mkdir -p /home/bitnami/logs/{mariadb,redmine}

docker-compose up -d

docker-compose ps

      Name                     Command               State           Ports         
-----------------------------------------------------------------------------------
bitnami_mariadb_1   /opt/bitnami/scripts/maria ...   Up      0.0.0.0:3306->3306/tcp
bitnami_redmine_1   /app-entrypoint.sh /run.sh       Up      0.0.0.0:3000->3000/tcp
```

打开 `ip:3000`，初始密码：`admin`/`admin`

redmine日志：`/home/bitnami/logs/redmine/production.log`

docker-compose方式部署 redmine 完成。

---

报错：

```a
[ActiveJob] [ActionMailer::DeliveryJob] [53686211-7f5f-4692-99e7-29745bd7608e] Email delivery error: execution expired
[ActiveJob] [ActionMailer::DeliveryJob] [53686211-7f5f-4692-99e7-29745bd7608e] Performed ActionMailer::DeliveryJob (Job ID: 53686211-7f5f-4692-99e7-29745bd7608e) from Async(mailers) in 30013.55ms
```

解决：

configuration.yml

```yaml
default:
  email_delivery:
    delivery_method: :smtp
    smtp_settings:
      ssl: true             #必须要加上 ssl:true
      address: "smtp.exmail.qq.com"
      port: 465
      domain: "exmail.qq.com"
      authentication: :login
      user_name: "xxx@qq.com"
      password: "xxxxxx"
```

对于邮箱地址是以https开头的，必须设置ssl参数设置为true。

---
