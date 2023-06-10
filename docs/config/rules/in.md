# 入站规则

## IN-TYPE

匹配流量入站的类型

```yaml
rules:
- IN-TYPE,INNER,proxy
```

### 支持的类型

HTTP/SOCKS/TUN/TPROXY/REDIR/INNER

!!! note
    INNER 为 providers 的下载请求

## IN-USER

匹配入站用户名

```yaml
rules:
- IN-USER,meta,DIERCT
```

## IN-NAME

匹配入站名称

```yaml
rules:
- IN-NAME,ss,PROXY
```



关于[入站](../listeners.md#_3)
