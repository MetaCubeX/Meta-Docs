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

[Общие поля](./index.md)

## username

Пользователь SSH

## password

Пароль SSH

## private-key

Содержимое ключа/путь к ключу

## private_key_passphrase

Пароль ключа

## host-key

Ключ хоста, оставьте пустым, чтобы принимать все

## host-key-algorithms

Алгоритмы ключа хоста 