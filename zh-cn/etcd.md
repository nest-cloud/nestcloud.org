# Etcd

## 安装

```shell
$ npm i --save @nestcloud/etcd etcd3
```

## 配置

```yaml
etcd:
  hosts: localhost:2379
```

## 注册

```typescript
import { Module } from '@nestjs/common';
import { EtcdModule } from '@nestcloud/etcd';

@Module({
  imports: [
      EtcdModule.register(),
  ],
})
export class ApplicationModule {}
```

## 如何使用

```typescript
import { Injectable, IEtcd } from '@nestjs/common';
import { InjectEtcd } from '@nestcloud/etcd';

@Injectable()
export class TestService {
  constructor(
      @InjectEtcd() private readonly etcd: IEtcd,
  ) {
  }
}
```

## API 文档

### class EtcdModule

#### static register\(options\): DynamicModule

注册 config 模块

| field | type | description |
| :--- | :--- | :--- |
| options.dependencies | string\[\] | NEST_BOOT |

其他配置请查看 [Etcd3](https://github.com/mixer/etcd3) 文档
