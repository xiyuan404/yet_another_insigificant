## 安装
[Home](https://www.mongodb.com/) Resources > Server Document#Installation > Community Edition
[docs](https://www.mongodb.com/) > Database Manual

通过包管理工具安装
```bash
# beer taps(beer-related term)
# brew tap: makes a clone of the repository at <user>/<repo> https://github.com/<user>/homebrew-<repo>
brew tap mongodb/brew
# update Homebrew and all existing formulae(配方)
brew update
#  specify the version when install mongodb
brew install mongodb-community@7.0
```
通过docker安装


The installation includes the following executables:
  The `mongod` server : `/urs/local/bin/mongod`
  The `mongos` sharded cluster query router: /urs/local/bin/mongos
  The MongoDB Shell, `mongosh`: `/urs/local/bin/mongosh`


## 服务系统变量配置(sys-vars-config)启动
> 常见配置文件名
> .ini(initiative-初始化)
> config(~/.ssh) tsconfig.json redis.conf(yml)
> .eslintrc, .prettierrc (runtime config 运行时配置)


> fix brew services
> `brew untap homebrew/services`
> `brew tap homebrew/services`
> `brew services`
```bash
➜ brew services list
Name              Status  User File
mongodb-community none
#  start the mongod process as a macOS service
➜ brew services start mongodb-community
# To stop a mongod running as a macOS service
brew services stop mongodb-community
# Connect and Use MongoDB
mongosh
```


配置项
| 参数(flag)      | 含义                             |
| :-------- | :------------------------------- |
| --dbpath  | 指定数据库文件的目录             |
| --port    | 端口 默认是27017 28017           |
| --fork    | 以后台守护的方式进行启动         |
| --logpath | 指定日志文件输出路径             |
| --config  | 指定一个配置文件                 |
| --auth    | 以安全方式启动数据库，默认不验证 |


### 配置文件路径
For Apple Silicon Processor
`configuration file`:/opt/homebrew/etc/mongod.conf
`log directory`: /opt/homebrew/var/log/mongodb
`data directory`: /opt/homebrew/var/mongodb

MongoDB configuration files use the YAML format.
```yaml
systemLog:
   destination: file
   path: "/opt/homebrew/var/log/mongodb/mongod.log"
   logAppend: true
processManagement:
   fork: true
storage:
  dbPath: /opt/homebrew/var/mongodb
net:
   bindIp: 127.0.0.1
   port: 27017
setParameter:
   enableLocalhostAuthBypass: false
security:
  authorization: enabled
```












导入导出数据


## mongo存储结构


| SQL term | MongoDB term | describe |
| -------- | ------------ | -------- |
| database | database     | 数据库   |
| table    | collection   |          |
| row      | document     |          |
| index      | index     |          |



## mongoose

orm
odm


## 在项目中使用mongoose

```bash
npm i mongoose -S
```

```js
const db = mongoose.createConnection("mongodb://usr:pass@ip:port/database", {
  useNewUrlParse: true,
  useUnifiedTopology: true
})
```
