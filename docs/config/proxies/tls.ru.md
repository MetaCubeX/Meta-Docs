# Конфигурация TLS

```{.yaml linenums="1"}
proxies:
- name: "tls-example"
  tls: true
  sni: example.com
  servername: example.com
  fingerprint: xxx
  alpn:
  - h2
  - http/1.1
  skip-cert-verify: true
  # certificate: xxxx
  # private-key: xxx
  client-fingerprint: chrome
  reality-opts:
    public-key: xxxx
    short-id: xxxx
    support-x25519mlkem768: true
  ech-opts:
    enable: true
    config: base64_encoded_config
```

## tls

Включить tls, применяется только к протоколам, использующим `tls`, протокол `trojan` принудительно включает

## sni/servername

Server Name Indication, в [`VMess`](./vmess.md)/[`VLESS`](./vless.md) - `servername`, если пусто, то используется адрес из поля `server`

## fingerprint

Отпечаток сертификата, применяется только к протоколам, использующим `tls`, можно получить с помощью 
```bash
openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem
```
В качестве альтернативы, вы можете получить отпечаток через «отпечаток SHA256» в разделе «Сертификат» в «Просмотрщике сертификатов» браузера Chrome.

!!! warning

    * В настоящее время гарантируется наличие отпечатков только для конечных сертификатов (т.е. сертификатов, содержащих имя SNI) в этом поле. Ввод отпечатков для других типов сертификатов (например, промежуточных или корневых сертификатов) приведет к неопределенному поведению.
    
    * Отпечаток в этом поле — это отпечаток всего сертификата, а не «отпечаток открытого ключа сертификата», определенный в HPKP. Пожалуйста, не путайте их.

## alpn

Список поддерживаемых протоколов согласования прикладного уровня, расположенных в порядке приоритета.

Если обе стороны поддерживают ALPN, выбранный протокол будет одним из этого списка, если нет взаимно поддерживаемых протоколов, соединение не будет установлено.

См. [Application-Layer Protocol Negotiation](https://en.wikipedia.org/wiki/Application-Layer_Protocol_Negotiation)

## skip-cert-verify

Пропустить проверку сертификата, применяется только к протоколам, использующим `tls`

## certificate

Если заполнено, включает [mTLS](https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/) (необходимо указать private-key). Содержимое — сертификат в формате PEM или путь к сертификату.

## private-key

Если заполнено, включает [mTLS](https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/) (необходимо указать certificate). Содержимое — закрытый ключ, соответствующий сертификату в формате PEM, или путь к закрытому ключу.

## client-fingerprint

Отпечаток клиента utls, применяется только к протоколам [`VMess`](./vmess.md)/[`VLESS`](./vless.md)/[`Trojan`](./trojan.md)/[`AnyTLS`](./anytls.md).

!!! note
    Возможные значения: `chrome`, `firefox`, `safari`, `iOS`, `android`, `edge`, `360`, `qq`, `random`. При выборе `random` генерируется отпечаток современного браузера на основе данных вероятности Cloudflare Radar.

## reality-opts

Настройки reality, если не пусто, включает reality

### reality-opts.public-key

Публичный ключ, соответствующий приватному ключу сервера reality

### reality-opts.short-id

Один из short id сервера 

### reality-opts.support-x25519mlkem768

Поддержка обмена ключами X25519-MLKEM768.

## ech-opts

Настройки ECH (Encrypted Client Hello), если `enable` установлено в `true`, то ECH будет включен.

### ech-opts.enable

Включает ECH (Encrypted Client Hello). Установка значения true включает ECH.

### ech-opts.config

Конфигурация ECH: Если поле пустое, используется разрешение DNS; в противном случае значение указывается в виде параметра ech, закодированного в base64 (Например, `AEn+DQBFKwAgACABWIHUGj4u+PIggYXcR5JF0gYk3dCRioBW8uJq9H4mKAAIAAEAAQABAANAEnB1YmxpYy50bHMtZWNoLmRldgAA`).

!!! info
    Вы можете использовать команду `mihomo generate ech-keypair test.com` для генерации совместимой пары самоподписанных конфигурационных ключей ECH как для сервера, так и для клиента. Пожалуйста, замените `test.com` на имя домена SNI, который вы хотите сделать доступным. Содержимое после `Config:` в выводе можно заполнить здесь, а содержимое после `Key:` следует заполнить в конфигурации ECH на стороне сервера (`ech-key` в списке слушателей mihomo).
