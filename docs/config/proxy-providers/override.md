```{.yaml linenums="1"}
proxy-providers:
  "hy":
    type: file
    path: ./proxy_providers/hy.yaml
    health-check: {enable: true, url: http://www.gstatic.com/generate_204, interval: 300}
    override:
      udp: true
      name-prefix: prefix
      name-suffix: suffix
```

## 覆写支持字段

[udp](../proxies/index.md#udp)

[up](../proxies/hysteria2.md)

[down](../proxies/hysteria2.md)

[skip-cert-verify](../proxies/index.md#udp)

[dialer-proxy](../proxies/index.md#dialer-proxy)

[interface-name](../proxies/index.md#interface-name)

[routing-mark](../proxies/index.md#routing-mark)

[ip-version](../proxies/index.md#ip-version)

### name-prefix

添加节点名称前缀

### name-suffix

添加节点名称后缀
