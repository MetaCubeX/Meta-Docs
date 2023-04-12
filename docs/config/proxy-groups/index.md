# 策略组

## 关于策略组

策略组是clash核心的功能之一

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
