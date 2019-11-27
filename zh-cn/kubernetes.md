# Kubernetes Client

The kubernetes client module for nestcloud.

[中文文档](https://github.com/nest-cloud/nestcloud/blob/master/docs/kuberentes.md)

## Installation

```bash
$ npm install --save @nestcloud/kubernetes
```

## 使用

### 使用外部 Kubernetes 集群

```typescript
import { Module } from '@nestjs/common';
import { KubernetesModule } from '@nestcloud/kubernetes';

@Module({
  imports: [
      KubernetesModule.register({kubeConfig: '/root/.kube/config'})
  ],
})
export class ApplicationModule {}
```

### 使用 InCluster 集群

```typescript
import { Module } from '@nestjs/common';
import { KubernetesModule } from '@nestcloud/kubernetes';

@Module({
  imports: [
      KubernetesModule.register()
  ],
})
export class ApplicationModule {}
```

### 使用 Kubernetes Client

```typescript
import { Injectable, IKubernetes } from '@nestjs/common';
import { InjectKubernetes } from '@nestcloud/kubernetes';

@Injectable()
export class TestService {
  constructor(@InjectKubernetes() private readonly client: IKubernetes) {}

  async getConfigMaps() {
      const result = await this.client.api.v1.namespaces('default').configmaps('test-configmap').get();
      console.log(result);
  }
}
```
