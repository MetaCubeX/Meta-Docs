# 预置代理组

## GLOBAL

默认填充所有代理组和代理节点

该策略组也可以在配置文件中自定义

但 web 面板以及部分客户端使用 GLOBAL 策略组内的 策略组顺序 进行排序

建议在自定义 GLOBAL 策略组时，书写完整当前配置所有的策略组 (除了 GLOBAL 本身)

未自定义 GLOBAL 且没有选择过时，GLOBAL 选中的是列表中的第一项 `DIRECT`。此时切换到 `global` 模式，所有流量都会直连。在 GLOBAL 中选好策略后，在 `rule` / `global` 模式之间切换不会改变这个选择

如需固定 GLOBAL 的初始选择，可以自定义 GLOBAL 并设置 [`default-selected`](./index.md#default-selected)：

```{.yaml linenums="1"}
proxy-groups:
  - name: GLOBAL
    type: select
    proxies: [proxy-a, proxy-b, DIRECT]
    default-selected: proxy-a
```
