# Change Log

## 0.6.5

1. bug fix

## 0.6.3

1. Etcd3 supports
2. Rename `consul-config` module to `config` module
3. Config module register must specific NEST_CONSUL or NEST_ETCD dependency
4. Rename `consul-service` module to `service` module
5. Service Module register must specific NEST_CONSUL or NEST_ETCD dependency
6. Service configuration adjusts

## 0.5.3

1. fix bugs

## 0.4.4

1. rename some modules name
- npm install @nestcloud/consul-loadbalance
+ npm install @nestcloud/loadbalance

- npm install @nestcloud/gateway
+ npm install @nestcloud/proxy

2. fix some bugs

## 0.3.17

1. use NestJS internal logger instead
2. fix import module with wrong path

## 0.3.13

1. Not crash app when register service fail, it will always retry until register succeed
2. Redis module support cluster

## 0.3.12

1. Proxy support set timeout to peer request by http header
2. Proxy support filters
3. Fix proxy init error when not set filter
4. New Redis module
5. New Brakes module

## 0.3.7

### Global

Update NestCloud dependencies.

### Consul Loadbalance

Fix make server node status unavailable when someone node is down and consul cluster is crashed.

### Gateway

Fix make server node status unavailable when someone node is down and consul cluster is crashed.


## 0.3.4

### Core

bug fix

### Logger

bug fix


## 0.2.4

### Core
1. Fixes NestFactory.createMicroservice or NestFactory.createApplicationContext type error

### Consul Loadbalance
1. Fixes server state reset when refresh services and servers

### Schedule

1. Supports dynamic schedule job
2. Uses NestJS dynamic module register
3. New UseLocker decorator
4. The Locker supports dependency injection, if use NestCloud.

# Grpc

A new component for loadbalance rpc invoke.


## 0.2.3

### All
1. Rewrite some codes

### Common
1. Add some interfaces

### Boot
1. Remove Bootstrap decorator
2. Use handlebars.js compiles the configurations

### Consul
1. Use WatchKV decorator instead ConsulKV

### Consul Config
1. Remove Configuration decorator
2. Use handlebars compiles the configurations
3. Remove internal expression, please use Boot module handlebars.js compiler instead
4. Remove retry configuration

### Consul Service
1. Update configuration's structure
2. Use some new function instead old function

### Consul Loadbalance
1. Fix custom lb rule, add a new configuration 'customRulePath'
2. Support load configurations from ConsulConfig, it supports dynamic upgrade.
3. Add Choose decorator

### Core
This is a new component, some other component's feature dependency it.

### Feign
1. Support load configurations from Boot or ConsulConfig
2. Use UseInterceptor decorator instead Interceptor decorator
3. Brakes feature support, Add UseBrakes, UseFallback, UseHealthCheck decorators
4. Fallback, HealthCheck, Interceptor are all support dynamic import and dependencies injection, such as NestJS Interceptors, Pipes

### Gateway
1. Support load configurations from ConsulConfig, it supports dynamic upgrade
2. Gateway instance add a new interface gateway.updateRoutes
