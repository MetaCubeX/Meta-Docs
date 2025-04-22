# Содержимое файла rule-providers

## classical

Тип `classical` поддерживает все типы [правил маршрутизации](../rules/index.md) (кроме rule-set/sub-rule).

=== "yaml"
    ```{.yaml linenums="1"}
    payload:
    - DOMAIN-SUFFIX,google.com
    - DOMAIN-KEYWORD,google
    - DOMAIN,ad.com
    - SRC-IP-CIDR,192.168.1.201/32
    - IP-CIDR,127.0.0.0/8
    - GEOIP,CN
    - DST-PORT,80
    - SRC-PORT,7777
    ```

=== "text"
    ```text
    DOMAIN-SUFFIX,google.com
    DOMAIN-KEYWORD,google
    DOMAIN,ad.com
    SRC-IP-CIDR,192.168.1.201/32
    IP-CIDR,127.0.0.0/8
    GEOIP,CN
    DST-PORT,80
    SRC-PORT,7777
    ```

## domain

Содержимое набора правил `domain` должно соответствовать [подстановочным знакам clash](../../handbook/syntax.ru.md#подстановочные-знаки-для-доменов).

=== "yaml"
    ```{.yaml linenums="1"}
    payload:
    - '.blogger.com'
    - '*.*.microsoft.com'
    - 'books.itunes.apple.com'
    ```

=== "text"
    ```text
    .blogger.com
    *.*.microsoft.com
    books.itunes.apple.com
    ```

## ipcidr

=== "yaml"
    ```{.yaml linenums="1"}
    payload:
    - '192.168.1.0/24'
    - '10.0.0.0.1/32'
    ```

=== "text"
    ```text
    192.168.1.0/24
    10.0.0.0.1/32
    ``` 