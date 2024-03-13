# SSH

```{.yaml linenums="1"}
proxies:
- name: "ssh-out"
  type: ssh

  server: 127.0.0.1
  port: 22
  username: root
  password: password
  private-key: key
  private-key-passphrase: key_password
  host-key:
  - "ssh-rsa AAAAB3NzaC1yc2EAA..."
  host-key-algorithms:
  - rsa
```

[通用字段](./index.md)

## username

SSH 用户

## password

SSH 密码

## private-key

密钥内容/路径

## private_key_passphrase

密钥密码

## host-key

主机密钥，留空接受所有

## host-key-algorithms

主机密钥算法
