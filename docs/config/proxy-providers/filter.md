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
    exclude-filter: "xxx"
    exclude-type: "ss|http"
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
    exclude-filter: "xxx"
    exclude-type: "Shadowsocks|Http"
```

## filter

筛选满足关键词或[正则表达式](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)的节点
```yaml
filter: "(?i)港|hk|hongkong|hong kong"
```



## exclude-filter
排除满足关键词或[正则表达式](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)的节点

```yaml
filter: "(?i)港|hk|hongkong|hong kong"
```


## exclude-type

不支持正则表达式，通过 `|` 分割，根据节点类型排除
注意，`proxy-groups` 与 `proxy-providers` 写法不同
