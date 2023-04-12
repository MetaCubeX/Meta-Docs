# GEO规则

## GEOSITE

域名集合,匹配集合内的域名,具体参考 [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community/tree/master/data)

```yaml
GEOSITE,google,proxy
GEOSITE,cn,DIRECT
```

## GEOIP

国家IP代码规则,匹配集合内相应的IP范围

```yaml
GEOIP,CN,DIRECT
GEOIP,LAN,DIRECT
```

### no-resolve

关于 [no-resolve](ipcidr.md#no-resolve)

```yaml
GEOIP,lan,DIRECT,no-resolve
```
