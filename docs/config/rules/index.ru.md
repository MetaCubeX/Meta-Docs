# Правила маршрутизации

```{.yaml linenums="1"}
rules:
- DOMAIN,ad.com,REJECT
- DOMAIN-SUFFIX,google.com,auto
- DOMAIN-KEYWORD,google,auto
- DOMAIN-WILDCARD,*.google.com,auto
- DOMAIN-REGEX,^abc.*com,PROXY
- GEOSITE,youtube,PROXY

- IP-CIDR,127.0.0.0/8,DIRECT,no-resolve
- IP-CIDR6,2620:0:2d0:200::7/32,auto
- IP-SUFFIX,8.8.8.8/24,PROXY
- IP-ASN,13335,DIRECT
- GEOIP,CN,DIRECT

- SRC-GEOIP,cn,DIRECT
- SRC-IP-ASN,9808,DIRECT
- SRC-IP-CIDR,192.168.1.201/32,DIRECT
- SRC-IP-SUFFIX,192.168.1.201/8,DIRECT

- DST-PORT,80,DIRECT
- SRC-PORT,7777,DIRECT

- IN-PORT,7890,PROXY
- IN-TYPE,SOCKS/HTTP,PROXY
- IN-USER,mihomo,PROXY
- IN-NAME,ss,PROXY

- PROCESS-PATH,/usr/bin/wget,PROXY
- PROCESS-PATH,C:\Program Files\Google\Chrome\Application\chrome.exe,PROXY
- PROCESS-PATH-WILDCARD,/usr/*/wget,PROXY
- PROCESS-PATH-REGEX,.*bin/wget,PROXY
- PROCESS-PATH-REGEX,(?i).*Application\\chrome.*,PROXY

- PROCESS-NAME,curl,PROXY
- PROCESS-NAME,chrome.exe,PROXY
- PROCESS-NAME,com.termux,PROXY
- PROCESS-NAME-WILDCARD,*telegram*,PROXY
- PROCESS-NAME-REGEX,curl$,PROXY
- PROCESS-NAME-REGEX,(?i)Telegram,PROXY
- PROCESS-NAME-REGEX,.*telegram.*,PROXY
- UID,1001,DIRECT

- NETWORK,udp,DIRECT
- DSCP,4,DIRECT

- RULE-SET,providername,proxy
- AND,((DOMAIN,baidu.com),(NETWORK,UDP)),DIRECT
- OR,((NETWORK,UDP),(DOMAIN,baidu.com)),REJECT
- NOT,((DOMAIN,baidu.com)),PROXY
- SUB-RULE,(NETWORK,tcp),sub-rule

- MATCH,auto
```

## Приоритет

Правила будут сопоставляться в порядке сверху вниз, причем правила в верхней части списка имеют более высокий приоритет, чем правила ниже.

!!! warning ""
    Если запрос для UDP, а прокси-узел не поддерживает UDP (например, если для узла `ss` не указано `udp: true`), сопоставление будет продолжено ниже.

### DOMAIN

Сопоставляет полное доменное имя.

### DOMAIN-SUFFIX

Сопоставляет суффикс домена.

Например, `google.com` соответствует `www.google.com`, `mail.google.com` и `google.com`, но не соответствует `content-google.com`.

### DOMAIN-KEYWORD

Сопоставляет по ключевым словам домена.

### DOMAIN-WILDCARD

Сопоставление с подстановочными знаками, поддерживает только подстановочные знаки `*` и `?`.

Здесь символ `*` соответствует нулю или более символам, а символ `?` соответствует ровно одному символу.

!!! note
    Обратите внимание, что используемые здесь подстановочные знаки отличаются от [подстановочных знаков формата Clash](../../handbook/syntax.md#_8) в других местах конфигурационного файла.

### DOMAIN-REGEX

Сопоставляет с использованием регулярных выражений для доменных имен.

### GEOSITE

Сопоставляет домены внутри Geosite; некоторый контент ссылается на [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community/tree/master/data).

### IP-CIDR & IP-CIDR6

Сопоставляет диапазоны IP-адресов; `IP-CIDR` и `IP-CIDR6` имеют одинаковый эффект, `IP-CIDR6` - это просто псевдоним.

### IP-SUFFIX

Сопоставляет диапазоны суффиксов IP.

### IP-ASN

Сопоставляет ASN IP-адреса.

### GEOIP

Сопоставляет код страны IP-адреса.

### SRC-GEOIP

Сопоставляет код страны исходного IP-адреса.

### SRC-IP-ASN

Сопоставляет ASN исходного IP-адреса.

### SRC-IP-CIDR

Сопоставляет диапазон исходных IP-адресов.

### SRC-IP-SUFFIX

Сопоставляет диапазон суффиксов исходных IP-адресов.

### DST-PORT

Сопоставляет целевой [диапазон портов](../../handbook/syntax.ru.md#диапазоны-портов) запроса.

### SRC-PORT

Сопоставляет исходный [диапазон портов](../../handbook/syntax.ru.md#диапазоны-портов) запроса.

### IN-PORT

Сопоставляет [входящий порт](../inbound/listeners/index.md#port), допускает [диапазоны портов](../../handbook/syntax.ru.md#диапазоны-портов).

### IN-TYPE

Сопоставляет [тип входящего соединения](../inbound/listeners/index.md#type).

### IN-USER

Сопоставляет имя пользователя [входящего соединения](../inbound/listeners/index.md), поддерживает несколько имен пользователей, разделенных `/`.

### IN-NAME

Сопоставляет [имя входящего соединения](../inbound/listeners/index.md#name).

### PROCESS-PATH

Сопоставляет по полному пути процесса.

### DOMAIN-WILDCARD

Используется сопоставление путей обработки с помощью символов подстановки, поддерживаются только символы подстановки `*` и `?`.

Здесь символ `*` соответствует нулю или более символам, а символ `?` соответствует ровно одному символу.

!!! note
    Обратите внимание, что используемые здесь подстановочные знаки отличаются от [подстановочных знаков формата Clash](../../handbook/syntax.md#_8) в других местах конфигурационного файла.

### PROCESS-PATH-REGEX

Сопоставляет с использованием регулярных выражений для пути процесса.

### PROCESS-NAME

Сопоставляет по имени процесса; на платформе `Android` может сопоставлять имена пакетов.

### DOMAIN-WILDCARD

Использует сопоставление по подстановочным знакам в именах процессов, поддерживая только подстановочные знаки `*` и `?`. На платформе Android также может сопоставлять имена пакетов.

Здесь символ `*` соответствует нулю или более символам, а символ `?` соответствует ровно одному символу.

!!! note
    Обратите внимание, что используемые здесь подстановочные знаки отличаются от [подстановочных знаков формата Clash](../../handbook/syntax.md#_8) в других местах конфигурационного файла.

### PROCESS-NAME-REGEX

Сопоставляет с использованием регулярных выражений для имени процесса; на платформе `Android` может сопоставлять имена пакетов.

### UID

Сопоставляет Linux USER ID.

### NETWORK

Сопоставляет `tcp` или `udp`.

### DSCP

Сопоставляет тег `DSCP` (только для входящих соединений tproxy UDP).

### RULE-SET

Ссылается на набор правил; требуется конфигурация [rule-providers](../rule-providers/index.md).

### AND & OR & NOT

`LOGIC_TYPE,((payload1),(payload2)),Proxy`

*payload1* и *payload2* - это типы правил и другие полезные данные, например, `DOMAIN,google.com`.

Логические правила требуют **аккуратного использования скобок**.

### SUB-RULE

Сопоставляет с [подправилами](../sub-rule.md); требуется аккуратное использование скобок.

### MATCH

Сопоставляет все запросы без условий.

## Дополнительные параметры

### no-resolve

Поддерживает только правила, касающиеся `целевого IP`.

Когда начинается сопоставление домена для правил `целевого IP`, mihomo запускает разрешение DNS, чтобы проверить, соответствует ли `целевой IP` домена правилам. Можно выбрать опцию `no-resolve`, чтобы пропустить разрешение DNS.

Если разрешение DNS было запущено при более раннем сопоставлении, оно все равно будет соответствовать правилам `целевого IP`, к которым добавлена опция `no-resolve`.

### src

Поддерживает только правила, касающиеся `целевого IP`.

Преобразует сопоставление `целевого IP` в сопоставление `исходного IP`. 
