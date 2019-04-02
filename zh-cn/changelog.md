# Change Log

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
