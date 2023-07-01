# 代理组筛选节点

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

## filter

筛选满足关键词或[正则表达式](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)的节点

```yaml
filter: "(?i)港|hk|hongkong|hong kong"
```

## exclude-filter

排除满足关键词或[正则表达式](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)的节点

```yaml
exclude-filter: "(?i)港|hk|hongkong|hong kong"
```

## exclude-type

排除节点类型

```yaml
exclude-type: "Shadowsocks|Http"
```

注意，`proxy-groups` 与 `proxy-providers` 写法不同，不支持正则表达式，通过 `|` 分隔
