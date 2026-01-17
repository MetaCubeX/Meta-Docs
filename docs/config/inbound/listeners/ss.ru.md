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
  # shadow-tls:
  #   enable: false
  #   version: 3 # v1/v2/v3
  #   password: password # v2
  #   users: # v3
  #     - name: 1
  #       password: password
  #   handshake:
  #     dest: test.com:443
  # kcp-tun:
  #   enable: false
  #   key: it's a secrect # pre-shared secret between client and server
  #   crypt: aes # aes, aes-128, aes-128-gcm, aes-192, salsa20, blowfish, twofish, cast5, 3des, tea, xtea, xor, none, null
  #   mode: fast # profiles: fast3, fast2, fast, normal, manual
  #   conn: 1 # set num of UDP connections to server
  #   autoexpire: 0 # set auto expiration time(in seconds) for a single UDP connection, 0 to disable
  #   scavengettl: 600 # set how long an expired connection can live (in seconds)
  #   ratelimit: 0 # set maximum outgoing speed (in bytes per second) for a single KCP connection, 0 to disable. Also known as packet pacing
  #   mtu: 1350 # set maximum transmission unit for UDP packets
  #   sndwnd: 128 # set send window size(num of packets)
  #   rcvwnd: 512 # set receive window size(num of packets)
  #   datashard: 10 # set reed-solomon erasure coding - datashard
  #   parityshard: 3 # set reed-solomon erasure coding - parityshard
  #   dscp: 0 # set DSCP(6bit)
  #   nocomp: false # disable compression
  #   acknodelay: false # flush ack immediately when a packet is received
  #   nodelay: 0
  #   interval: 50
  #   resend: 0
  #   sockbuf: 4194304 # per-socket buffer in bytes
  #   smuxver: 1 # specify smux version, available 1,2
  #   smuxbuf: 4194304 # the overall de-mux buffer in bytes
  #   framesize: 8192 # smux max frame size
  #   streambuf: 2097152 # per stream receive buffer in bytes, smux v2+
  #   keepalive: 10 # seconds between heartbeats
```

## [Общие поля](./index.md)

## Настройка протокола

### cipher

Метод шифрования

| Метод                          | Длина пароля |
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

| Метод | Формат пароля                        |
| ---- | ------------------------------------ |
| none | /                                    |
| 2022 | `openssl rand --base64 <длина пароля>` |
| другие | любая строка                           |

### udp

Прослушивать ли UDP 
