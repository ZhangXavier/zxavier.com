---
title: "MongoDB 刚上道"
subtitle: ""
date: 2021-01-21T16:22:08+08:00
lastmod: 2021-01-21T16:22:08+08:00

description: "由创建MongoDB环境到一些命令"

tags: [MongoDB,Docker,数据库]
categories: [代码之道]

draft: false
---
<!--more-->

## MongoDB 环境

### 使用 Docker 创建 MongoDB 环境

无论任何一个数据库，第一步终是要有一个开发环境的。在容器化技术如此普及的今天，使用 `Docker` 来创建环境，无疑是最好的选择。首先，创建一个 `./mongodb-docker-compose.yml`

``` yml ./mongodb-docker-compose.yml
version: '3.7'

services:
  mongo:
    image: mongo
    container_name: mongo
    restart: always
    ports:
      - "ports:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: nameroot
      MONGO_INITDB_ROOT_PASSWORD: pwroot
    volumes:
      - mongo-data:/data/db

  mongo-express:
    image: mongo-express
    container_name: mongo-express
    restart: always
    ports:
      - "127.0.0.1:ports:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: nameroot
      ME_CONFIG_MONGODB_ADMINPASSWORD: pwroot
      ME_CONFIG_BASICAUTH_USERNAME: username
      ME_CONFIG_BASICAUTH_PASSWORD: password
      ME_CONFIG_OPTIONS_EDITORTHEME: idea

volumes:
  mongo-data:
```

通过编排文件运行

``` shell
docker-compose -f mongodb-docker-compose.yml up -d
```

## 对 MongoDB 的操作

进入 `docker`

``` shell
docker exec -it mongodb
```

进入 `Mongo`

``` shell
mongo
```

创建一个可以创建普通用户的管理员

``` mongo
use admin
switched to db admin
db.auth('nameroot','pwroot')
> db.createUser({
    user: 'nameUserAdmin',
    pwd: 'pwuseradmin',
    roles: [
        {
            role: 'userAdminAnyDatabase',
            db:'admin'
        }
    ]})
}
db.auth('nameUserAdmin','pwuseradmin')
```

鉴权后要进入目标数据库创建用户

``` mongo
use test
db.createUser(
  {
     user:"test1",
     pwd: "test1",
     roles: [{ role: "readWrite", db: "test"}]
  }
)
```

验证

``` mongo
use test
> db.auth('test1','test1')
1
```

### mongo 相关知识

#### 内建的角色

- 数据库用户角色：`read`、`readWrite`;
- 数据库管理角色：`dbAdmin`、`dbOwner`、`userAdmin`；
- 集群管理角色：`clusterAdmin`、`clusterManager`、`clusterMonitor`、`hostManager`；
- 备份恢复角色：`backup`、`restore`；
- 所有数据库角色：`readAnyDatabase`、`readWriteAnyDatabase`、`userAdminAnyDatabase`、`dbAdminAnyDatabase`
- 超级用户角色：`root` #这里还有几个角色间接或直接提供了系统超级用户的访问（`dbOwner`、`userAdmin`、`userAdminAnyDatabase`）
- 内部角色：`__system`

#### 内建角色说明

- `read`：允许用户读取指定数据库；
- `readAnyDatabase`：只在 `admin` 数据库中可用，赋予用户所有数据库的读权限；
- `readWrite`：允许用户读写指定数据库；
- `readWriteAnyDatabase`：只在 `admin` 数据库中可用，赋予用户所有数据库的读写权限；
- `dbAdmin`：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问`system.profile`；
- `dbAdminAnyDatabase`：只在 `admin` 数据库中可用，赋予用户所有数据库的 `dbAdmin`权限；
- `clusterAdmin`：只在 `admin` 数据库中可用，赋予用户所有分片和复制集相关函数的管理权限；
- `userAdmin`：允许用户向 `system.users` 集合写入，可以找指定数据库里创建、删除和管理用户；
- `userAdminAnyDatabase`：只在 `admin` 数据库中可用，赋予用户所有数据库的`userAdmin`权限；
- `root`：只在 `admin` 数据库中可用。超级账号，超级权限；

#### 主要命令

``` shell
show dbs # 显示数据库列表
show collections # 显示当前数据库中的集合（类似关系数据库中的表）
show users # 显示用户
use <db name> # 切换当前数据库，如果数据库不存在则创建数据库。
db.help() # 显示数据库操作命令，里面有很多的命令
db.foo.help() # 显示集合操作命令，同样有很多的命令，foo指的是当前数据库下，一个叫foo的集合，并非真正意义上的命令
db.foo.find() # 对于当前数据库中的foo集合进行数据查找（由于没有条件，会列出所有数据）
db.foo.find( { a : 1 } ) # 对于当前数据库中的 foo 集合进行查找，条件是数据中有一个属性叫 a，且 a 的值为 1
```

MongoDB没有创建数据库的命令，但有类似的命令。 如：如果你想创建一个 `myTest` 的数据库，先运行 `use myTest` 命令，之后就做一些操作（如 `db.createCollection(‘user’)）` 这样就可以创建一个名叫 `myTest` 的数据库。

#### 其他命令

``` shell
db.dropDatabase() # 删除当前使用数据库
db.cloneDatabase("127.0.0.1") # 将指定机器上的数据库的数据克隆到当前数据库
db.copyDatabase("mydb", "temp", "127.0.0.1") #将本机的 mydb 的数据复制到 temp 数据库中
db.repairDatabase() # 修复当前数据库
db.getName() # 查看当前使用的数据库，也可以直接用 db
db.stats() # 显示当前 db 状态
db.version() # 当前 db 版本
db.getMongo() ＃ 查看当前 db 的链接机器地址
db.serverStatus() # 查看数据库服务器的状态
```
