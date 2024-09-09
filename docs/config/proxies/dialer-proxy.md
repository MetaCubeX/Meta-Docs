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
```

指定当前 `proxies` 通过 `dialer-proxy` 建立网络连接，值可以为[策略组](../proxy-groups/index.md)/[出站代理](../proxies/index.md)的 `name`

上述示例中，ss1 通过 ss2 建立连接

当通过 ss1 代理时，就组成了一条 `内核>ss2>ss1>目标` 的代理链