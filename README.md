# EcshopX本地开发环境说明

EcshopX 采用前后端分离的方式开发，代码结构由后端api和多个前端组成。

后端api由lumen开发，部署时包含了web、scheduler、worker和websocket。

前端包含管理端、小程序、PC端和H5端。

本环境目前只包含了web和管理的部署，后续会根据需求将其他环境加入。

## 准备
开始前，需要确保本地已经安装 docker。然后先将 docker 开发环境的脚本 git clone 到本地：
```shell
git clone git@github.com:shopex/ecshopx-dev-docker.git
cd ecshopx-dev-docker
```

## 第一步：初始化代码
将管理端的代码 git clone 到 ecshopx-admin 目录中

将api端的代码 git clone 到 ecshopx-api 目录中
## 第二步：启动开发环境
在ecshopx-dev-docker下执行
```
docker-compose up
```

## 第三步：创建数据库
访问<http://localhost:8004> 账号：root 密码：root 。进入phpmyadmin，创建项目数据库ecshopx。也可使用其他数据库工具创建数据库，连接地址为：localhost:8806

访问<http://localhost:7474> 账号：neo4j 密码：neo4j 进入neo4j，修改密码为123456，不修改密码，无法通过api链接到neo4j

## 第三步：配置安装api

### 1、配置`.env`

* 修改数据库配置
* 修改redis配置
* 修改neo4j配置

```
DB_CONNECTION=default
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=ecshopx
DB_USERNAME=root
DB_PASSWORD=root

NEO4J_DEFAULT_PROTOCOL=http
NEO4J_DEFAULT_HOST=neo4j
NEO4J_DEFAULT_PORT=7474
NEO4J_DEFAULT_USERNAME=neo4j
NEO4J_DEFAULT_PASSWORD=123456

REDIS_CLIENT=predis
REDIS_HOST=redis
REDIS_PASSWORD=123456
```

### 2、composer&&migrate


```shell
#查看web容器名称
docker ps | grep web
#进入容器
docker exec -it ecshopx-dev-docker_espier-bloated-web_1 sh 
```

进入容器之后：

```
composer install
./artisan doctrine:migrations:migrate
```


## 访问环境

在完成以上几步之后，就可以访问开发环境了，具体访问地址如下：
> 在访问环境前，请先确认管理端代码是否编译完成,可通过 docker logs ecshopx-dev-docker_ecshop-admin-build_1 查看编译进度

| 项目 | 地址 | 
| - | - |
| 管理端 | <http://127.0.0.1:8080/> | 
| 后端api | <http://127.0.0.1:8080/api> | 
| 微信授权 | <http://127.0.0.1:8080/wechatAuth> | 
| phpmyadmin | <http://127.0.0.1:8004> | 
| neo4j | <http://127.0.0.1:7474> |


