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
  # name-cert-verify: example.com
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
    # query-server-name: xxx.com
  shadow-tls-opts: 
    version: 3 
    password: shadow-tls-password
  restls-opts:
    password: restls-password
    version-hint: tls13
  jls-opts:
    username: jls-user
    password: jls-password
  tlsmirror-opts:
    primary-key: MDEyMzQ1Njc4OWFiY2RlZjAxMjM0NTY3ODlhYmNkZWY=
    explicit-nonce-ciphersuites: [
      156, 157, 158, 159, 160, 161, 162, 163, 164, 165, 166, 167, 168, 169, 170, 171,
      172, 173, 49195, 49196, 49197, 49198, 49199, 49200, 49201, 49202, 49290, 49291,
      49293, 49316, 49317, 49318, 49319, 49320, 49321, 49322, 49323, 49324, 49325,
      49326, 49327, 52392, 52393, 52394, 52395, 52396, 52397, 52398
    ]
    defer-instance-derived-write-time:
      base-nanoseconds: 0
      uniform-random-multiplier-nanoseconds: 0
    transport-layer-padding:
      enabled: false
    connection-enrolment:
      primary-egress-outbound: ""
    sequence-watermarking-enabled: false
    embedded-traffic-generator:
      steps:
        - name: example
          host: example.com
          path: /
          method: GET
          headers:
            - name: User-Agent
              value: mihomo
            - name: Accept
              values:
                - text/html
                - application/xhtml+xml
          connection-ready: true
          connection-recall-exit: true
          h2-do-not-wait-for-download-finish: false
          wait-time:
            base-nanoseconds: 1000000000
            uniform-random-multiplier-nanoseconds: 0
          next-step:
            - weight: 1
              goto-location: 0
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

    * При вводе конечного сертификата (т.е. сертификата, содержащего имя SNI) проверяется только отпечаток сертификата, отправленного сервером; дополнительные проверки не выполняются.

    * При вводе отпечатка сертификатов других типов (например, промежуточных или корневых сертификатов) будет проверено, была ли цепочка сертификатов, отправленная сервером, выдана этим сертификатом. Начиная с версии 1.19.20, также должно выполняться требование SNI/имени сервера.
    
    * Отпечаток в этом поле — это отпечаток всего сертификата, а не «отпечаток открытого ключа сертификата», определенный в HPKP. Пожалуйста, не путайте их.

## alpn

Список поддерживаемых протоколов согласования прикладного уровня, расположенных в порядке приоритета.

Если обе стороны поддерживают ALPN, выбранный протокол будет одним из этого списка, если нет взаимно поддерживаемых протоколов, соединение не будет установлено.

См. [Application-Layer Protocol Negotiation](https://en.wikipedia.org/wiki/Application-Layer_Protocol_Negotiation)

## skip-cert-verify

Пропустить проверку сертификата, применяется только к протоколам, использующим `tls`

## name-cert-verify

Необязательно. Изменяет только целевое имя DNSName для проверки сертификата, не меняя SNI.

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

!!! warning
    Из-за намеренно [несовместимого поведения](https://github.com/XTLS/Xray-core/commit/af7eb68028732a8ee3c0e5d6ab2b8a657bb2e770) xray-core мы не будем рассматривать совместимость с версиями xray v26.7.11+. Если это не работает, замените сервер (например, [нативный listener mihomo](../inbound/listeners/index.md), sing-box или старую версию xray-core) или используйте другой протокол.

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

### ech-opts.query-server-name

Этот параметр необязателен; если он не пуст, он используется для указания доменного имени при разрешении через DNS.

## shadow-tls-opts

Требуется включить `tls: true`. Использует `sni` / `servername` из общих настроек в качестве ShadowTLS SNI.

### shadow-tls-opts.version

Поддерживаются версии `v1` / `v2` / `v3`. По умолчанию используется `v2`, если поле оставлено пустым.

### shadow-tls-opts.password

Пароль ShadowTLS.

## restls-opts

Требуется включить `tls: true`. Использует `sni` / `servername` из общих настроек в качестве Restls SNI.

### restls-opts.password

Пароль Restls.

### restls-opts.version-hint

Подсказка версии TLS. Возможные значения: `tls12` / `tls13`.

### restls-opts.restls-script

Необязательный скрипт трафика Restls после рукопожатия.

## jls-opts

Требуется включить `tls: true`. Использует `sni` / `servername` из общих настроек в качестве JLS SNI.

### jls-opts.username

Имя пользователя JLS.

### jls-opts.password

Пароль JLS.

## tlsmirror-opts

Когда `tls` установлен в `true`, наличие `tlsmirror-opts` включает tlsmirror. TLS carrier, используемый tlsmirror, берет `servername`, `alpn`, `skip-cert-verify`, `name-cert-verify`, `fingerprint`, `certificate`, `private-key`, `client-fingerprint` и `ech-opts` из того же outbound. Если `servername` пустой, используется `server`.

!!! note
    Сейчас включение tlsmirror поддерживает только VMess. Не используйте его с другими протоколами.

### tlsmirror-opts.primary-key

Обязательно, base64-кодирование 32-байтового основного ключа.

### tlsmirror-opts.explicit-nonce-ciphersuites

Наборы шифров с явным nonce для носителя TLS 1.2.

### tlsmirror-opts.defer-instance-derived-write-time

Задержка перед первой записью.

### tlsmirror-opts.transport-layer-padding

Настройки заполнения транспортного уровня.

### tlsmirror-opts.connection-enrolment

v2ray-совместимые настройки регистрации соединения. Для исходящего VMess в mihomo `primary-egress-outbound` обычно оставляют пустым.

#### tlsmirror-opts.connection-enrolment.primary-ingress-outbound

Тег контрольного исходящего соединения на стороне inbound для регистрации соединения. Обычно используется для соответствия v2ray-совместимой серверной конфигурации.

#### tlsmirror-opts.connection-enrolment.primary-egress-outbound

Тег контрольного исходящего соединения на стороне outbound для регистрации соединения. Для исходящего VMess в mihomo обычно оставляют пустым; в v2ray можно указать отдельный контрольный outbound tag.

### tlsmirror-opts.sequence-watermarking-enabled

Включать ли sequence watermarking.

### tlsmirror-opts.embedded-traffic-generator

Генерирует дополнительный HTTP carrier-трафик. Протокол определяется `alpn`. Если `steps` не настроен, генератор не включается.

#### tlsmirror-opts.embedded-traffic-generator.steps

Список шагов HTTP carrier-трафика. Шаги выполняются по порядку, если `next-step` не выбрал другой шаг.

#### tlsmirror-opts.embedded-traffic-generator.steps.name

Имя шага, используется только как идентификатор.

#### tlsmirror-opts.embedded-traffic-generator.steps.host

Целевой host HTTP-запроса.

#### tlsmirror-opts.embedded-traffic-generator.steps.path

Путь HTTP-запроса.

#### tlsmirror-opts.embedded-traffic-generator.steps.method

Метод HTTP-запроса.

#### tlsmirror-opts.embedded-traffic-generator.steps.headers

Список HTTP-заголовков. В каждом элементе `name` задает имя заголовка, `value` задает одно значение, а `values` задает несколько значений.

#### tlsmirror-opts.embedded-traffic-generator.steps.connection-ready

Передать proxy-соединение после завершения этого шага.

#### tlsmirror-opts.embedded-traffic-generator.steps.connection-recall-exit

Завершить carrier-трафик после закрытия proxy-соединения.

#### tlsmirror-opts.embedded-traffic-generator.steps.h2-do-not-wait-for-download-finish

Если carrier-протокол — `h2`, не ждать завершения чтения тела ответа.

#### tlsmirror-opts.embedded-traffic-generator.steps.wait-time

Время ожидания после завершения шага. Поля совпадают с [tlsmirror-opts.defer-instance-derived-write-time](#tlsmirror-optsdefer-instance-derived-write-time).

#### tlsmirror-opts.embedded-traffic-generator.steps.next-step

Взвешенный список кандидатов для следующего шага. `weight` — вес, `goto-location` — индекс целевого шага.
