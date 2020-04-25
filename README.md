# docker-apollo
本镜像主要是引用于[idoop/docker-apollo](https://github.com/idoop/docker-apollo)的镜像，在此镜像的基础上加入了EUREKA注册地址是Docker内网IP转为可以配置为宿主机IP的功能，为了解决本地IDEA环境开发调试，无法连接到meta server的问题

## Apollo Version: 

- [`1.6.0`](https://github.com/ctripcorp/apollo/releases/tag/v1.6.0) `latest`

## 我做了什么

* DockerFile

  ```bash
  # # 加入EUREKA IP可以指定为宿主机
  ENV EUREKA_IP ""
  ```

* bootstrap.yml

  ```yml
  # 在adminservice和configservice的bootstrap.yml加入了配置
  eureka:
    instance:
      ip-address: ${eureka.instance.ip-address}
  ```

* start.sh

  ```bash
  # 在adminservice和configservice的start.sh加入了-Deureka.instance.ip-address=$EUREKA_IP的启动参数
  if [ "$DS_URL"x != x ]
  then
      export JAVA_OPTS="$JAVA_OPTS -Dspring.datasource.url=$DS_URL -Dspring.datasource.username=$DS_USERNAME -Dspring.datasource.password=$DS_PASSWORD -Deureka.instance.ip-address=$EUREKA_IP"
  fi
  ```

## 启动前确认对应的数据库已建立

[创建数据库指导](https://github.com/ctripcorp/apollo/wiki/%E5%88%86%E5%B8%83%E5%BC%8F%E9%83%A8%E7%BD%B2%E6%8C%87%E5%8D%97#21-%E5%88%9B%E5%BB%BA%E6%95%B0%E6%8D%AE%E5%BA%93)

## 使用 Docker Compose 启动
假设想要开启Protal/Dev/Fat,那么建立一个`docker-compose.yaml`文件,内容大致如下所示,只需将mysql数据库地址与库名以及账号密码替换为自己的,并配置好数据库:
``` yaml
version: '1.1'
services:
  apollo:
    image: michaelliu/apollo:latest
    volumes:
      - ./logs:/opt
    environment:
      PORTAL_DB: jdbc:mysql://192.168.31.192:3306/ApolloPortalDB?characterEncoding=utf8
      PORTAL_DB_USER: root
      PORTAL_DB_PWD: password
      
      # 开启dev环境, 默认端口: config 8080, admin 8090
      DEV_DB: jdbc:mysql://192.168.31.192:3306/ApolloConfigDB?characterEncoding=utf8
      DEV_DB_USER: root
      DEV_DB_PWD: password
```