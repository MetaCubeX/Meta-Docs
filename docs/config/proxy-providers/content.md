# 代理集内容

一个合法的代理集合必须包含 `proxies:`字段

```yaml
proxies:
  - name: "ss1"
    type: ss
    server: server
    port: 443
    cipher: chacha20-ietf-poly1305
    password: "password"

  - name: "ss2"
    type: ss
    server: server
    port: 443
    cipher: chacha20-ietf-poly1305
    password: "password"
```

## 额外支持

meta 额外支持 ray 系代理软件的 URI 以及 base64 后的 URI,例如

### URI

```yaml
ss://YWVzLTI1Ni1nY206bWV0YUAxMjcuMC4wLjE6NDQz#home
vmess://eyJhZGQiOiIxMjcuMC4wLjEiLCJhaWQiOiIwIiwiYWxwbiI6IiIsImZwIjoiIiwiaG9zdCI6IiIsImlkIjoiMTIyMzQ1Njc4OSIsIm5ldCI6InRjcCIsInBhdGgiOiIiLCJwb3J0IjoiNDQzIiwicHMiOiJ2bWVzcyIsInNjeSI6ImF1dG8iLCJzbmkiOiIiLCJ0bHMiOiIiLCJ0eXBlIjoibm9uZSIsInYiOiIyIn0=
```

base 64

```yaml
c3M6Ly9ZV1Z6TFRJMU5pMW5ZMjA2YldWMFlVQXhNamN1TUM0d0xqRTZORFF6I2hvbWUKdm1lc3M6Ly9leUpoWkdRaU9pSXhNamN1TUM0d0xqRWlMQ0poYVdRaU9pSXdJaXdpWVd4d2JpSTZJaUlzSW1ad0lqb2lJaXdpYUc5emRDSTZJaUlzSW1sa0lqb2lNVEl5TXpRMU5qYzRPU0lzSW01bGRDSTZJblJqY0NJc0luQmhkR2dpT2lJaUxDSndiM0owSWpvaU5EUXpJaXdpY0hNaU9pSjJiV1Z6Y3lJc0luTmplU0k2SW1GMWRHOGlMQ0p6Ym1raU9pSWlMQ0owYkhNaU9pSWlMQ0owZVhCbElqb2libTl1WlNJc0luWWlPaUl5SW4wPQ==
```

!!! note
YAML / URI / base64 不可写在同一文件,URI / base64 不需要 `proxies:`字段,直接书写即可
