# dialer-proxy

```{.yaml linenums="1"}
proxies:
- name: "ss1"
  dialer-proxy: dialer
  ...

- name: "ss2"
  ...

proxy-groups:
- name: dialer
  type: select
  proxies:
  - ss2

rules:
  - MATCH,ss1
```

指定当前 `proxies` 通过 `dialer-proxy` 建立网络连接，值可以为[策略组](../proxy-groups/index.md)/[出站代理](../proxies/index.md)的 `name`

上述示例中，ss1 通过 ss2 建立连接

当通过 ss1 代理时，就组成了一条 `內核 ---ss1--> ss2包裝器 ===ss2==> ss2服務端 ---ss1--> ss1服務端 --> 目标` 的代理链

最终表现：

  * 浏览器访问 IP 查询网站时只会显示 ss1 的 IP（即目标网站不知道 ss2 的存在）
  * 内核对外看起来只是在访问 ss2（即你的宽带运营商不知道 ss1 的存在）
  * 只有 ss2 的服务端知道在访问 ss1（ss2 的服务端也只知道在访问 ss1，并不知道在访问什么目标网站）

## 常见实例

### 通过订阅节点中转自己的 VPS 落地

```{.yaml linenums="1"}
proxies:
- name: "ss1"
  dialer-proxy: dialer
  ...

proxy-providers:
  provider1:
    type: http
    url: "http://test.com"

proxy-groups:
- name: dialer
  type: select
  use:
  - provider1

rules:
  - MATCH,ss1
```

这里将订阅地址填入 provider1 中，将你自己 VPS 中搭建的节点填入 ss1 中即可，此时通过浏览器访问时显示的是 ss1 的 IP

!!! note
    没有特殊需求的情况下，在自己被中转的 VPS 落地中搭建的节点请勿选择任何 udp 类协议如 hy2/tuic/wg，以及带有 tls 伪装类协议如 reality/shadowtls，您的订阅节点可能不能正常通过这些协议，这里建议选择最简单的 ss aead 或者 vmess 协议

### 通过特定 socks 连接订阅节点

```{.yaml linenums="1"}
proxies:
- { name: "socks1", type: "socks" }

proxy-providers:
  provider1:
    type: http
    url: "http://test.com"
    override:
      dialer-proxy: socks1

proxy-groups:
- name: select1
  type: select
  use:
  - provider1

rules:
  - MATCH,select1
```

该实例适用于需要通过一个特定 socks 才能访问外网的情况（如内外网隔离环境），将环境提供的 socks 配置填入 socks1，将订阅地址填入 provider1 中，此时通过浏览器访问时显示的是订阅中节点的 IP

!!! note
    这里只是示范了 dialer-proxy 的相关配置，您可能还需要更多的配置才能保证订阅下载，dns 解析等流程同样通过该 socks

## relay 迁移

relay 类型的 proxy-group 已经被废弃，而 proxy-group 并不直接支持 dialer-proxy，因此针对部分使用场景，给出参考方案

### relay 中包含多个 select

有如下配置

```{.yaml linenums="1"}
proxies:
  - { name: "proxy1", type: "socks" }
  - { name: "proxy2", type: "socks" }
  - { name: "proxy3", type: "socks" }
  - { name: "proxy4", type: "socks" }
proxy-groups:
  - { name: "relay-proxy", type: relay, proxies: ["select1", "select2"] }
  - { name: "select1", type: select, proxies: ["proxy1", "proxy2"] }
  - { name: "select2", type: select, proxies: ["proxy3", "proxy4"] }
rules:
  - MATCH,relay-proxy
```

迁移到 dialer-proxy 方案时，需要将 proxy3 和 proxy4 定义到 proxy-provider 中，并通过 override 设置该 provider 中所有 proxy 的 dialer-proxy，如下所示：

```{.yaml linenums="1"}
proxies:
  - { name: "proxy1", type: "socks" }
  - { name: "proxy2", type: "socks" }
proxy-groups:
  - { name: "select1", type: select, proxies: ["proxy1", "proxy2"] }
  - { name: "select2", type: select, use: ["provider1"] }
proxy-providers:
  provider1:
    type: inline
    override:
      dialer-proxy: select1
    payload:
      - { name: "proxy3", type: "socks" }
      - { name: "proxy4", type: "socks" }
rules:
  - MATCH,select2
```
