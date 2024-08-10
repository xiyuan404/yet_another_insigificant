
[redid official](redis.io) docs > develop with redis

## 安装和启动
```bash
brew install redis
brew services start redis
```
redis cli
/usr/local/bin/redis-cli
redis server
/usr/local/bin/redis-server

配置文件
/usr/local/etc/redis.conf
/usr/local/etc/redis.conf.default


brew services restart redis


RedisClientOptions


## redis in Node.js

## 发布订阅

## 事务

## 备份与恢复


## 安全


```bash
127.0.0.1:6379> CONFIG get requirepass
1) "requirepass"
2) ""
127.0.0.1:6379> CONFIG set requirepass "xiyuan"
OK
127.0.0.1:6379> CONFIG get requirepass
1) "requirepass"
2) "xiyuan"
127.0.0.1:6379> AUTH xiyuan
OK
```
