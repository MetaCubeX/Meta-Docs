# **代理端口**

### http(s) 代理端口

```yaml
port: 7890
```

### socks4/4a/5 代理端口

```yaml
socks-port: 7891
```

### http,socks入口的验证用户名，密码

```yaml
authentication: 
  - "username:password"
```

### 设置跳过验证的IP段

```yaml
skip-auth-prefixes: 
  - 127.0.0.1/8
  - ::1/128
```

### 混合端口

!!! node
    混合端口是一个特殊的端口, 它同时支持 HTTP(S) 和 SOCKS5 协议. 您可以使用任何支持 HTTP 或 SOCKS 代理的程序连接到这个端口

```yaml
mixed-port: 7892
```

### 透明代理端口
!!! note

    redirect端口仅限 Linux(Android) 以及 macOS 适用，tproxy 端口仅限 linux(Android) 适用

redirect 透明代理端口，仅能代理 TCP 流量

```yaml
redir-port: 7893
```

tproxy 透明代理端口，可代理 TCP 与 UDP 流量

```yaml
tproxy-port: 7894
```
