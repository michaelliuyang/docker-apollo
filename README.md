## Apollo Version: 

- [`1.6.0`](https://github.com/ctripcorp/apollo/releases/tag/v1.6.0) `latest`

## 启动前确认对应的数据库已建立

[创建数据库指导](https://github.com/ctripcorp/apollo/wiki/%E5%88%86%E5%B8%83%E5%BC%8F%E9%83%A8%E7%BD%B2%E6%8C%87%E5%8D%97#21-%E5%88%9B%E5%BB%BA%E6%95%B0%E6%8D%AE%E5%BA%93)

## 使用 Docker Compose 启动
假设想要开启Protal/Dev/Fat,那么建立一个`docker-compose.yaml`文件,内容大致如下所示,只需将mysql数据库地址与库名以及账号密码替换为自己的,并配置好数据库:
``` yaml
version: '2.2'
services:
  apollo:
    image: michaelliu/apollo:latest
    container_name: apollo
    restart: always
    ports:
      - "8070:8070"
      - "8090:8090"
      - "8080:8080"
    volumes:
      - ./logs:/opt
    environment:
      # EUREKA IP 配置为宿主机
      DEV_LB: 192.168.31.192
      PORTAL_DB: jdbc:mysql://192.168.31.192:3306/ApolloPortalDB?characterEncoding=utf8
      PORTAL_DB_USER: root
      PORTAL_DB_PWD: password

      DEV_DB: jdbc:mysql://192.168.31.192:3306/ApolloConfigDB?characterEncoding=utf8
      DEV_DB_USER: root
      DEV_DB_PWD: password

```