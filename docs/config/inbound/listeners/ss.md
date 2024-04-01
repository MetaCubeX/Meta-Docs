# ShadowSocks

```{.yaml linenums="1"}
listeners:
- name: ss-in
  type: shadowsocks
  port: 10001
  listen: 0.0.0.0
  cipher: 2022-blake3-aes-256-gcm
  password: vlmpIPSyHH6f4S8WVPdRIHIlzmB+GIRfoH3aNJ/t9Gg=
  udp: true
```

## [通用字段](./index.md)

## 协议配置

### cipher

加密方法

| 方法                          | 密码长度 |
| ----------------------------- | -------- |
| 2022-blake3-aes-128-gcm       | 16       |
| 2022-blake3-aes-256-gcm       | 32       |
| 2022-blake3-chacha20-poly1305 | 32       |
| none                          | /        |
| aes-128-gcm                   | /        |
| aes-192-gcm                   | /        |
| aes-256-gcm                   | /        |
| chacha20-ietf-poly1305        | /        |
| xchacha20-ietf-poly1305       | /        |

### password

| 方法 | 密码格式                             |
| ---- | ------------------------------------ |
| none | /                                    |
| 2022 | `openssl rand --base64 <密码长度>` |
| 其他 | 任意字符串                           |

### udp

是否监听 UDP
