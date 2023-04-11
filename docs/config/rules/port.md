# 端口规则

## **`DST-PORT`**

目标端口规则,匹配请求的目标端口

<pre><code><strong>DST-PORT,22,DIRECT
</strong></code></pre>

## `SRC-PORT`

`来源端口规则,`匹配请求来源的端口

```
SRC-PORT,7890,DIRECT
```

## 端口范围写法

可使用/匹配多个端口,使用-匹配端口范围,可混合书写

### 示例

匹配114和514端口

```
DST-PORT,114/514,DIRECT
```

匹配114到514端口

```
DST-PORT,114/514,DIRECT
```

匹配114和233以及514到1919端口

```
DST-PORT,114/233/514-1919,DIRECT
```
