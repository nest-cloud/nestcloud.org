# 配置清单

```yaml
consul:
  host: localhost
  port: 8500

etcd:
  hosts: localhost:2379

service:
  id: null
  name: service
  port: 3000
  # 多网卡或者容器场景下需要手动指定服务IP，否则可能会自动选择内网IP
  discoveryHost: localhost
  healthCheck:
    # for consul
    timeout: 1s
    interval: 10s
    # for etcd
    ttl: 20
  maxRetry: 5
  retryInterval: 5000

config:
  # 表达式支持获取yaml以及系统变量中的数据
  key: config__${{ consul.service.name }}__${{ NODE_ENV }}

# 支持通过 config 加载
proxy:
  routes:
    - id: user
      uri: lb://multicloud-user-service
    - id: pay
      uri: http://pay.example.com
      
# 支持通过 config 加载
feign:
  axios:
    timeout: 1000
    
# 支持通过 config 加载
loadbalance:
  ruleCls: RandomRule
  
logger:
  level: info
  transports:
    - transport: console
      colorize: true
      datePattern: YYYY-MM-DD h:mm:ss
      label: nestcloud
    - transport: file
      name: info
      filename: info.log
      datePattern: YYYY-MM-DD h:mm:ss
      label: nestcloud
      maxSize: 104857600
      json: false
      maxFiles: 10
    - transport: dailyRotateFile
      label: nestcloud
      filename: info.log
      datePattern: YYYY-MM-DD-HH
      zippedArchive: true
      maxSize: 20m
      maxFiles: 14d
```
