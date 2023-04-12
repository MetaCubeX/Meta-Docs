# 筛选代理集

```yaml
proxy-providers:
  provider1:
    type: http
    path: ./meta1.yaml
    url: http://example.com/files/meta1.yaml
    interval: 3600
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
  provider2:
    type: http
    path: ./meta2.yaml
    url: http://example.com/files/meta2.yaml
    interval: 3600
    filter: "(?i)港|hk|hongkong|hong kong"
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
  
 proxy-groups: 
  - name: Proxy
    type: select
    use:
      - provider1
      - provider2
    filter: "(?i)港|hk|hongkong|hong kong"
```

## filter

支持使用关键词、[正则表达式](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)筛选节点
