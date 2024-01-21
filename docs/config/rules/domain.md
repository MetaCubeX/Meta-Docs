## DOMAIN

域名规则，如果请求的域完全匹配，则会匹配上此规则

```{.yaml linenums="1"}
rules:
- DOMAIN,google.com,auto
```

## DOMAIN-SUFFIX

域名后缀规则，如果请求的域名后缀匹配，则会匹配上此规则

例：“google.com”匹配“www.google.com”、“mail.google.com”和“google.com”, 但不匹配“content-google.com”

```{.yaml linenums="1"}
rules:
- DOMAIN-SUFFIX,google.com,auto
```

## DOMAIN-KEYWORD

域名关键词规则，如果请求的域名中包含关键字，则会匹配上此规则

```{.yaml linenums="1"}
rules:
- DOMAIN-KEYWORD,ad,REJECT
```
