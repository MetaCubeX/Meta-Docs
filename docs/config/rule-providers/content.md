### classical

=== "yaml"
    ```yaml
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

### domain

`domain`类规则集合内容通配应遵守[clash通配符](../index.md#_7)

=== "yaml"
    ```yaml
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

### ipcidr


=== "yaml"
    ```yaml
    payload:
    - '192.168.1.0/24'
    - '10.0.0.0.1/32'
    ```

=== "text"
    ```text
    192.168.1.0/24
    10.0.0.0.1/32
    ```

