# buildrun-emqx-backend-mysql
EMQX  client connection and message save to MySQL

emqx 客户端连接状态，消息持久化，规则触发动作到MySQL 插件

### 编译发布插件

1、clone emqx-rel 项目

> git clone https://github.com/emqx/emqx-rel.git

2.rebar.config 添加依赖

```erl
{deps,
   [ {buildrun_emqx_backend_mysql, {git, "https://github.com/goBuildRun/buildrun-emqx-backend-mysql.git", {branch, "master"}}}
   , ....
   ....
   ]
}

```

3.rebar.config 中 relx 段落添加

```erl
{relx,
    [...
    , ...
    , {release, {emqx, git_describe},
       [
         {buildrun_emqx_backend_mysql, load},
       ]
      }
    ]
}
```
4.编译

> make

### config配置

File: etc/emqx_backend_mysql.conf

```
## MySQL Server
backend.mysql.pool1.server = 127.0.0.1:3306

## MySQL Pool Size
backend.mysql.pool1.pool_size = 8
## MySQL Username
backend.mysql.pool1.user = root
## MySQL Password
backend.mysql.pool1.password = public
## MySQL Database
backend.mysql.pool1.database = mqtt
```

### mqtt_client.sql, mqtt_msg.sql

mqtt_client.sql, mqtt_msg.sql到你的数据库中

### 加载插件

> ./bin/emqx_ctl plugins load emqx_backend_mysql

或者编辑

data/loaded_plugins

> 添加 {emqx_backend_mysql, true}.

注意：这种方式适用emqx未启动之前

### 使用

此插件会把public发布的消息保存到mysql中，但并不是全部。

需要在发布消息的参数中 retain 值设置为 true。 这样这条消息才会被保存在mysql中

eg：

```json
{
  "topic": "t/a",
  "payload": "hello",
  "qos": 1,
  "retain": true,
  "client_id": "mqttjs_8b3b4182ae"
}
```

### 最后

有什么问题和功能需求都可以给我提issue，欢迎关注。

### License

Apache License Version 2.0
