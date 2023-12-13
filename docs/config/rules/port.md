## **DST-PORT**

目标端口规则，匹配请求的目标端口

```yaml
rules:
- DST-PORT,22,DIRECT
```

## **SRC-PORT**

来源端口规则，匹配请求来源的端口

```yaml
rules:
- SRC-PORT,7890,DIRECT
```

## 端口范围写法

可使用/匹配多个端口，使用-匹配端口范围，可混合书写

### 示例

匹配 114 和 514 端口

```yaml
rules:
- DST-PORT,114/514,DIRECT
```

匹配 114 到 514 端口

```yaml
rules:
- DST-PORT,114-514,DIRECT
```

匹配 114 和 233 以及 514 到 1919 端口

```yaml
rules:
- DST-PORT,114/233/514-1919,DIRECT
```
