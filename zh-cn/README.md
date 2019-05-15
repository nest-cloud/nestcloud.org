
[travis-image]: https://api.travis-ci.org/nest-cloud/nestcloud.svg?branch=master
[travis-url]: https://travis-ci.org/nest-cloud/nestcloud
[linux-image]: https://img.shields.io/travis/nest-cloud/nestcloud/master.svg?label=linux
[linux-url]: https://travis-ci.org/nest-cloud/nestcloud

# NestCloud

<p align="left">
    <a href="https://www.npmjs.com/~nestcloud" target="_blank"><img src="https://img.shields.io/npm/v/@nestcloud/core.svg" alt="NPM Version"/></a>
    <a href="https://www.npmjs.com/~nestcloud" target="_blank"><img src="https://img.shields.io/npm/l/@nestcloud/core.svg" alt="Package License"/></a>
    <a href="https://www.npmjs.com/~nestcloud" target="_blank"><img src="https://img.shields.io/npm/dm/@nestcloud/core.svg" alt="NPM Downloads"/></a>
    <a href="https://travis-ci.org/nest-cloud/nestcloud" target="_blank"><img src="https://travis-ci.org/nest-cloud/nestcloud.svg?branch=master" alt="Travis"/></a>
    <a href="https://travis-ci.org/nest-cloud/nestcloud" target="_blank"><img src="https://img.shields.io/travis/nest-cloud/nestcloud/master.svg?label=linux" alt="Linux"/></a>
    <a href="https://coveralls.io/github/nest-cloud/nestcloud?branch=master" target="_blank"><img src="https://coveralls.io/repos/github/nest-cloud/nestcloud/badge.svg?branch=master" alt="Coverage"/></a>
</p>

基于 Consul 的 NodeJS 微服务解决方案，各组件使用 Typescript 语言 和 NestJS 框架编写。

## 安装

```bash
npm install --save @nestcloud/core @nestcloud/common @nestcloud/boot @nestcloud/consul @nestcloud/consul-service @nestcloud/consul-config @nestcloud/consul-loadbalance @nestcloud/feign @nestcloud/logger @nestcloud/schedule 
```


## 主要组件

### [Boot](zh-cn/boot.md)

在应用启动的时候读取应用本地配置以及系统环境变量。


### [Consul](zh-cn/consul.md)

Consul 模块


### [Consul-Config](zh-cn/consul-config.md)

从配置中心读取和监听配置。


### [Consul-Service](zh-cn/consul-service.md)

服务注册和服务发现。


### [Consul-Loadbalance](zh-cn/consul-loadbalance.md)

软件实现的负载均衡，主要为 http 调用提供服务。


### [Feign](zh-cn/feign.md)

Http 客户端，支持负载均衡并且支持用 Decorator 编写。


### [Grpc](zh-cn/grpc.md)

提供 Grpc，并且支持负载均衡。


### [Memcached](zh-cn/memcached.md)

Memcached 模块。


### [Schedule](zh-cn/schedule.md)

支持分布式运行的定时任务库，支持 Decorator 编写。


### [Logger](zh-cn/logger.md)

基于 winston@2.x 实现的日志模块。

### [Validations](zh-cn/validations.md)

Http 请求参数验证模块。


## 快速开始

main.ts

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { NestLogger } from '@nestcloud/logger';
import { NestCloud } from '@nestcloud/core';

async function bootstrap() {
    const app = NestCloud.create(await NestFactory.create(AppModule, {
        logger: new NestLogger({
            path: __dirname,
            filename: `config.yaml`
        }),
    }));

    await app.listen(NestCloud.global.boot.get('consul.service.port', 3000));
}

bootstrap();
```

app.module.ts

```typescript
import { Module } from '@nestjs/common';
import { 
    NEST_BOOT, 
    NEST_CONSUL_LOADBALANCE, 
    NEST_CONSUL_CONFIG
} from '@nestcloud/common';
import { BootModule } from '@nestcloud/boot';
import { ConsulModule } from '@nestcloud/consul';
import { ConsulConfigModule } from '@nestcloud/consul-config';
import { ConsulServiceModule } from '@nestcloud/consul-service';
import { LoadbalanceModule } from '@nestcloud/consul-loadbalance';
import { FeignModule } from '@nestcloud/feign';
import { LoggerModule } from '@nestcloud/logger';
import { TerminusModule } from '@nestjs/terminus';

@Module({
    imports: [
        LoggerModule.register(),
        BootModule.register(__dirname, `config.yaml`),
        ConsulModule.register({ dependencies: [NEST_BOOT] }),
        ConsulConfigModule.register({ dependencies: [NEST_BOOT] }),
        ConsulServiceModule.register({ dependencies: [NEST_BOOT] }),
        LoadbalanceModule.register({ dependencies: [NEST_BOOT] }),
        FeignModule.register({ dependencies: [NEST_BOOT, NEST_CONSUL_LOADBALANCE] }),
        TerminusModule.forRootAsync({
            useFactory: () => ({ endpoints: [{ url: '/health', healthIndicators: [] }] }),
        }),
    ]
})
export class AppModule {
}
```


## Starter

使用 [NestCloud-Starter](https://github.com/nest-cloud/nestcloud-starter) 快速开始创建你的项目.


## 例子

[Samples](https://github.com/nest-cloud/nestcloud/samples)

## 谁在使用 NestCloud

![焱融云](https://nestcloud.org/_media/who-used/yanrong.svg)


## 保持联系

- Author - [miaowing](https://github.com/miaowing)

## License

  NestCloud is [MIT licensed](https://github.com/nest-cloud/nestcloud/LICENSE).
