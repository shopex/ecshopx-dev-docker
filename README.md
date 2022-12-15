# EcshopX本地开发环境说明

EcshopX 采用前后端分离的方式开发，代码结构由后端api和多个前端组成。

后端api由lumen开发，部署时包含了web、scheduler、worker。

前端包含管理端、小程序、PC端和H5端。

## 版本说明
当前为单机版本，需装多机版，请访问一下链接: 

[多机版](https://github.com/shopex/ecshopx-dev-docker/tree/develop/multiMachine)

## 准备
开始前，需要确保本地已经安装 docker。然后先将 docker 开发环境的脚本 git clone 到本地：
```shell
git clone https://github.com/shopex/ecshopx-dev-docker.git
cd ecshopx-dev-docker
```

## 第一步：初始化代码
将管理端和API端的代码放到对应的目录中：

如果，你刚刚拿到代码还没有放到 git 中，可以直接将代码复制到到对应的目录中：
* `管理端`的代码复制到 `ecshopx-admin` 目录
* `API端`的代码复制到 `ecshopx-api` 目录
* `PC端`的代码复制到 `ecshopx-pc` 目录
* `H5端`的代码复制到 `ecshopx-vshop` 目录

如果，你已经将代码放到 git 中，可以这样操作：
```
git clone {管理端 git 地址}  ecshopx-admin
git clone {API端 git 地址}  ecshopx-api
git clone {PC端 git 地址}  ecshopx-pc
git clone {H5端 git 地址}  ecshopx-vshop
```
完成后目录结构如下：
```shell
ecshopx-dev-docker
    ├── config
    ├── ecshopx-admin
    │   ├── ... 
    │   ├── .env
    │   └── README.md
    ├── ecshopx-api
    │   ├── app  
    │   ├── bootstrap
    │   ├── config
    │   ├── ...
    │   ├── src
    │   └── .env
    ├── ecshopx-pc
    │   ├── ... 
    │   ├── .env
    │   └── README.md
    ├── ecshopx-vshop
    │   ├── ... 
    │   ├── .env
    │   └── README.md
    ├── docker-compose.yml
    └── README.md
```
### 配置前端 api 地址
在前端代码的 `.env` 中配置后端 api 地址：

```js
BASE_API: '"api"',
```

### 编译前端代码
在docker所在主机安装好node，然后在本地按照前端代码包里面的README中的步骤编译代码

## 第二步：启动开发环境
在ecshopx-dev-docker下执行
```
docker-compose up
```

> 第一次启动时，需要通过 docker pull 拉去镜像到本地，所以需要等待一段时间

## 第三步：配置数据库

### 创建 MySQL 数据库
为保证安装环境的统一性，我们使用本环境自带的 `phpmyadmin` 来创建数据库：

* 一、访问 <http://127.0.0.1:8083> 输入账号密码: root root，登录 phpmyadmin 

* 二、创建项目数据库 `ecshopx` ，字符集选用 `utf8mb4_general_ci`

## 第四步：配置安装

### 1、配置 `.env`
代码中不包含`.env` 文件，可以将 `.env.example` 复制改名为 `.env`，
其路径为`ecshopx-dev-docker/ecshopx-api/.env`。

如果你完全按照第二步来配置数据库，以下配置可以直接复制替换 `.env` 原有配置。
* 修改数据库配置
```
DB_CONNECTION=default
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=ecshopx
DB_USERNAME=root
DB_PASSWORD=root
```
* 修改redis配置
```
REDIS_CLIENT=predis
REDIS_HOST=redis
REDIS_PORT=6379
REDIS_PASSWORD=123456
```

### 2、migrate

由于 API 的环境在 docker 的容器内，所以开发时，需要进入到容器中来执行相关命令。

#### 进入容器
```shell
docker exec -it ecshopx-dev-docker_ecshopx-api_1 sh 
```
进入容器之后，会自动进入代码目录 `/data/httpd`，所以可以直接运行命令：

#### 初始化数据库表
```
php artisan doctrine:migrations:migrate
```

## 访问环境

在完成以上几步之后，就可以访问开发环境了，具体访问地址如下：

| 项目 | 地址 | 
| - | - |
| 管理端 | <http://127.0.0.1:8080/> | 
| 后端api | <http://127.0.0.1:8080/api> | 
| phpmyadmin | <http://127.0.0.1:8083> | 
