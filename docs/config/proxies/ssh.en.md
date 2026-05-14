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

[Common fields](./index.md)

## username

SSH user.

## password

SSH password.

## private-key

Key content or path.

## private_key_passphrase

Key passphrase.

## host-key

Host key. Leave empty to accept all.

## host-key-algorithms

Host key algorithms.
