# 筛选代理集

```yaml
proxy-providers:
  hk:
    type: http
    path: ./meta.yaml
    url: http://example.com/files/meta.yaml
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
      - hk
```

## filter

支持使用关键词、[正则表达式](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)筛选节点
