```yaml

 proxy-groups: 
  - name: Proxy
    type: select
    use:
      - provider1
      - provider2
    filter: "(?i)港|hk|hongkong|hong kong"
    exclude-filter: "xxx"
    exclude-type: "Shadowsocks|Http"
```
