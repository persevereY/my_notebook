# macos 安装各种中间件的命令

## nginx

#### 启动nginx

```bash
nginx
```

#### 停止nginx

```shell
nginx -s reload
nginx -s stop
```

## 一些文件路径

- 安装目录
  
  ```shell
  /usr/local/Cellar/nginx/1.x.x
  ```

- 配置文件路径
  
  ```shell
  /usr/local/etc/nginx/nginx.conf
  ```

- 日志文件路径
  
  ```shell
  /usr/local/var/log/nginx $ ls -lrt
  total 80
  -rw-r--r--  1 staff  admin  29646  3 19 14:45 access.log
  -rw-r--r--  1 staff  admin   6430  3 19 19:11 error.log
  ```

## Redis

### 启动redis

```shell
redis-server /usr/local/etc/redis.conf  #alias 设置rediss
```

### 停止redis

```shell
 launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
 #停止redis自启动
```



## Zookeeper

## RabbitMq


