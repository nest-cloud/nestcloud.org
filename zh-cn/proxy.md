# API 代理

Proxy 是基于 http-proxy 实现的一个简单 API 代理模块

## 安装

```bash
npm install @nestcloud/gateway --save
```

## 注册模块

```typescript
import { Module } from '@nestjs/common';
import { ProxyModule } from "@nestcloud/proxy";
import { NEST_BOOT } from '@nestcloud/common';

@Module({
    imports: [
        ProxyModule.register({dependencies: [NEST_BOOT]}), // or NEST_CONFIG
    ]
})
export class AppModule {
}
```

## 配置

```yaml
proxy:
  routes:
    - id: user
      uri: lb://nestcloud-user-service
    - id: pay
      uri: https://example.com/pay
```

## 如何使用

```typescript
import { All, Controller, Param, Req, Res } from "@nestjs/common";
import { Request, Response } from 'express';
import { Proxy, InjectProxy } from "@nestcloud/proxy";

@Controller('/proxy/:service')
export class GatewayController {
    constructor(
        @InjectProxy() private readonly proxy: Proxy,
    ) {
    }

    @All()
    do(@Req() req: Request, @Res() res: Response, @Param('service') id) {
        this.proxy.forward(req, res, id);
    }
}
```

## 过滤器

Proxy 内置了 `AddRequestHeaderFilter` 和 `AddResponseHeaderFilter` 两个过滤器，如果你想使用自己的过滤器，需要实现 `IFilter`
接口，然后调用 `registerFilter(filter: IFilter)` 注册你的过滤器

```typescript
class CustomFilter implements IFilter {
    before(request: IRequest, response: IResponse): boolean | Promise<boolean> {
        return undefined;
    }

    error(error: ProxyErrorException, request: IRequest, response: IResponse) {
    }

    getName(): string {
        return "CustomFilter";
    }

    request(proxyReq: ClientRequest, request: IRequest, response: IResponse) {
    }

    response(proxyRes: IncomingMessage, request: IRequest, response: IResponse) {
    }
}
```

## 注意

使用该模块需要禁用 body parser 中间件，否则 put post 请求会 pending。

```typescript
const app = await NestFactory.create(AppModule, { bodyParser: false });
```

## API 文档

### class ProxyModule

#### static register\(options: IGatewayOptions = {}, proxy?: IProxyOptions\): DynamicModule

注册 proxy 模块

| field | type | description |
| :--- | :--- | :--- |
| options.dependencies | string\[\] | NEST_BOOT, NEST_CONFIG, NEST_LOADBALANCE |
| options.routes | IRoute[] | 路由转发规则 |
| proxy | IProxyOptions | 略，详情请查看 http-proxy 文档 |

### class Proxy

#### updateRoutes(routes: IRoute[], sync: boolean = true): void;

更新路由规则列表，如果 sync 为 false，则不会同步更新到 Consul KV，在某一时刻，更新过的路由列表会被 Consul KV 中的覆盖掉。

#### registerFilter(filter: IFilter)

注册自定义过滤器

