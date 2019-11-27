# 配置中心

Config 以 Consul，Kubernetes ConfigMap 或 Etcd 作为微服务的配置中心，从 Consul KV，Kubernetes ConfigMap 或 Etcd 读取所需的配置并支持对配置内容改动的监听。

## 安装

```bash
# consul backend
$ npm i --save @nestcloud/consul consul @nestcloud/config
# etcd backend
$ npm i --save @nestcloud/etcd etcd3 @nestcloud/config
# kubernetes backend
$ npm i --save @nestcloud/config
```

## 注册模块

### 1. Consul KV

```typescript
import { Module } from '@nestjs/common';
import { ConsulModule } from '@nestcloud/consul';
import { ConfigModule } from '@nestcloud/config';
import { BootModule } from '@nestcloud/boot';
import { NEST_BOOT, NEST_CONSUL } from '@nestcloud/common';

@Module({
  imports: [
      ConsulModule.register({dependencies: [NEST_BOOT]}),
      BootModule.register(__dirname, 'bootstrap.yml'),
      ConfigModule.register({dependencies: [NEST_BOOT, NEST_CONSUL]}),
  ],
})
export class ApplicationModule {}
```

### 2. Kubernetes ConfigMap

```typescript
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestcloud/config';
import { BootModule } from '@nestcloud/boot';
import { NEST_BOOT, NEST_KUBERNETES } from '@nestcloud/common';

@Module({
  imports: [
      BootModule.register(__dirname, 'bootstrap.yml'),
      ConfigModule.register({dependencies: [NEST_BOOT, NEST_KUBERNETES]}),
  ],
})
export class ApplicationModule {}
```

### 3. Etcd

```typescript
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestcloud/config';
import { EtcdModule } from '@nestcloud/etcd';
import { BootModule } from '@nestcloud/boot';
import { NEST_BOOT, NEST_ETCD } from '@nestcloud/common';

@Module({
  imports: [
      BootModule.register(__dirname, 'bootstrap.yml'),
      EtcdModule.register({dependencies: [NEST_BOOT]}),
      ConfigModule.register({dependencies: [NEST_BOOT, NEST_ETCD]})
  ],
})
export class ApplicationModule {}
```

## 配置

config.key 对 Consul 和 Etcd 作为存储后端的时候有效 config.key, config.namespace, config.path 只对 Kubernetes 作为存储后端的时候有效

```yaml
config:
  key: nestcloud-conf
  namespace: default
  path: config.yaml
```

### Consul KV 或 Etcd 配置例子

```yaml
user:
  info:
    name: 'test'
```

### Kubernetes ConfigMap 配置例子

```yaml
apiVersion: v1
data:
  config.yaml: |-
    user:
      info:
        name: 'test'

kind: ConfigMap
metadata:
  name: nestcloud-conf
  namespace: default
```

## 注入 Config Client

```typescript
import { Injectable } from '@nestjs/common';
import { InjectConfig, ConsulConfig } from '@nestcloud/config';

@Injectable()
export class TestService {
    constructor(
        @InjectConfig() private readonly config: ConsulConfig,
    ) {
    }
    
    test() {
        this.config.get('info', 'default');
    }
}
```

## 注入 Value

```typescript
import { Injectable } from '@nestjs/common';
import { ConfigValue } from '@nestcloud/config';

@Injectable()
export class TestService {
    @ConfigValue('info', 'default')
    private readonly info: string;
}
```

## API 文档

### class ConfigModule

#### static register\(options\): DynamicModule

注册 config 模块

| field | type | description |
| :--- | :--- | :--- |
| options.dependencies | string\[\] | NEST_BOOT, NEST_CONSUL, NEST_KUBERNETES |
| options.key | string | consul kv 的 key，或者 Kubernetes ConfigMap 的名称 |
| options.namespace | string | Kubernetes ConfigMap 所在的命名空间 |
| options.path | string | Kubernetes ConfigMap 的路径 |

### class Config

#### get\(path?: string, defaults?: any\): any

获取配置

| field | type | description |
| :--- | :--- | :--- |
| path | string | 获取指定路径的配置，如果不指定则获取所有 |
| defaults | any | 如果指定路径的配置不存在则返回默认值 |

#### getKey\(\): string

获取当前使用的 key

#### watch\(path: string, callback&lt;T extends any&gt;: \(configs: T\) =&gt; void\): void

监听配置内容变化

| field | type | description |
| :--- | :--- | :--- |
| path | string | 获取指定配置的路径 |
| callback | \(configs\) =&gt; void | 配置数据发生变化的回调函数 |

#### async set\(path: string, value: any\): void

修改配置中心对应的配置数据

| field | type | description |
| :--- | :--- | :--- |
| path | string | 待修改配置的路径 |
| value | any | 待修改配置的内容 |

### 装饰器

#### InjectConfig\(\): PropertyDecorator

注入 ConsulConfig 对象。

#### ConfigValue\(path: string, defaultValue?: any\): PropertyDecorator

自动为类的属性赋值，并支持动态更新。



