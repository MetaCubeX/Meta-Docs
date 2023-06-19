# 策略组

## 关于策略组

策略组是 Clash 核心的功能之一，可在策略组中添加代理节点、代理集

## 示例

!!! note
这只是一个示例，请不要照搬

```yaml
proxy-groups:
  - name: "proxy"
    type: select
    disable-udp: true
    filter: "HK|TW"
    proxies:
      - DIRECT
      - ss
      - vmess
    use:
      - provider1
      - provider1
```
