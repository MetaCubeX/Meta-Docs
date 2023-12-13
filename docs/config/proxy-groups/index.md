## 示例

!!! note
    仅作为示例，请根据自己需求合理配置

```yaml
proxy-groups:
  - name: "proxy"
    type: select
    disable-udp: true
    proxies:
      - DIRECT
      - ss
      - vmess
    use:
      - provider1
      - provider1
```
