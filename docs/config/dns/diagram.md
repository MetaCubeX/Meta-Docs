# DNS 流程图

```mermaid
graph TD;
  A[访问网站] --> B(Clash DNS)
  B --> C[使用 Redir-host]
  B --> D[使用 FakeIP]
  D --> E[FakeIP 返回 IP 地址]
  E --> F[向目标 Web 服务器发出请求]
```
