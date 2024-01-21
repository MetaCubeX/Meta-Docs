---
description: 用户可以单独将一些代理放入特定文件中，通过引用该文件，用户可以快速将这些相同的代理填充到不同的策略组中
---
## 示例

```{.yaml linenums="1"}
proxy-providers:
  provider1:
    type: http
    url: ""
    path: ./proxy_providers/provider1.yaml
    interval: 3600
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
      expected-status: 204

  provider2:
    type: file
    path: ./proxy_providers/provider2.yaml
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
```

### name

如 `provider1`, 为 provider 的 name,不能重复

### type

provider 类型，可选 `http/file`

### url

类型为 `http`是则需要配置

### path

文件路径，不可重复,可选，不填写时会使用MD5作为此文件的文件名

由于安全问题，此路径将限制只允许在 HomeDir（有启动参数 -d 配置） 中，如果想存储到任意位置配置环境变量 `SKIP_SAFE_PATH_CHECK=1`

### interval

更新 provider 的时间，单位为秒

### health-check

健康检查（测试延迟）

#### enable

是否启用，可选 `true/false`

#### url

健康检查地址，推荐使用以下地址之一：

Cloudflare:

```
https://cp.cloudflare.com/generate_204
```

Google：

```
http://www.gstatic.com/generate_204
https://www.gstatic.com/generate_204
```

#### interval

健康检查间隔时间，单位为秒

#### expected-status

参阅 [期望状态](../proxy-groups/index.md#expected-status)