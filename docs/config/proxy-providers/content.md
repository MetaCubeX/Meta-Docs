# proxy-providers 文件内容

=== "yaml"
    ```{.yaml linenums="1"}
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

=== "uri"
    ```{.yaml linenums="1"}
    ss://YWVzLTI1Ni1nY206bWV0YUAxMjcuMC4wLjE6NDQz#home
    vmess://eyJhZGQiOiIxMjcuMC4wLjEiLCJhaWQiOiIwIiwiYWxwbiI6IiIsImZwIjoiIiwiaG9zdCI6IiIsImlkIjoiMTIyMzQ1Njc4OSIsIm5ldCI6InRjcCIsInBhdGgiOiIiLCJwb3J0IjoiNDQzIiwicHMiOiJ2bWVzcyIsInNjeSI6ImF1dG8iLCJzbmkiOiIiLCJ0bHMiOiIiLCJ0eXBlIjoibm9uZSIsInYiOiIyIn0=
    ```

=== "base64"
    ```{.text linenums="1"}
    c3M6Ly9ZV1Z6TFRJMU5pMW5ZMjA2YldWMFlVQXhNamN1TUM0d0xqRTZORFF6I2hvbWUKdm1lc3M6Ly9leUpoWkdRaU9pSXhNamN1TUM0d0xqRWlMQ0poYVdRaU9pSXdJaXdpWVd4d2JpSTZJaUlzSW1ad0lqb2lJaXdpYUc5emRDSTZJaUlzSW1sa0lqb2lNVEl5TXpRMU5qYzRPU0lzSW01bGRDSTZJblJqY0NJc0luQmhkR2dpT2lJaUxDSndiM0owSWpvaU5EUXpJaXdpY0hNaU9pSjJiV1Z6Y3lJc0luTmplU0k2SW1GMWRHOGlMQ0p6Ym1raU9pSWlMQ0owYkhNaU9pSWlMQ0owZVhCbElqb2libTl1WlNJc0luWWlPaUl5SW4wPQ==
    ```

!!! note
    base64 的 uri 通常为提供商提供给 v2ray/xray 的订阅链接内容

    `YAML`/`uri`/`base64`不可写在同一文件,`uri`/`base64`不需要`proxies:`字段,直接书写即可
