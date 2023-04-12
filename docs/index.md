---
hide:
  - navigation
  - toc
---
# 简介

[**Clash.Meta**](https://github.com/MetaCubeX/Clash.Meta/tree/Meta) **是一个基于开源** [**Clash**](https://github.com/Dreamacro/clash) **项目的二次开发版本，增加了一些独有特性！**

文档仍在修订中，欢迎 [PR](https://github.com/MetaCubeX/Meta-Docs/)。

**使用 Clash.Meta Kernel 前，请悉知：**

> * 原 Clash 是一款全平台的支持多协议分流的代理软件，其项目地址为  [**GitHub Project**](https://github.com/Dreamacro/clash)；
> * 本 Wiki 主要介绍 Clash.Meta 的特色功能，原 Clash 特性请查看 [**Clash Wiki** ](https://lancellc.gitbook.io/clash/)**；**
> * 本 Wiki 基于最新的 [**Meta Alpha**](https://github.com/MetaCubeX/Clash.Meta/tree/Alpha) 分支介绍，如果您的 Meta 低于此版本，可能会有部分功能不支持；
> * Meta 核心支持所有原开源核心的全部特性，支持原 Clash Premium 核心部分特性。

### Clash.Meta 主要特性

#### **代理模块：**

* 支持出站传输协议 VLESS XTLS / Trojan XTLS
* 支持出站传输协议 Hysteria
* 支持 PASS ( 跳过 ) 规则
* 主动健康检测 urltest/fallback（基于 tcp 握手，限定时间内多次失败会主动触发健康检测使用节点）
* 支持策略组正则筛选
* 允许 Provider 请求过 Clash
* Proxy-Providers 支持直接使用 V2rayN 等客户端的普通订阅
* Relay 代理链支持 UDP over TCP
* TCP 连接并发

#### 规则模块

* 支持规则 GEOSITE
* 支持入站类型规则 IN-TYPE
* 支持规则集 RULE-SET
* 支持规则 SRC-PORT 和 DST-PORT 的多端口条件
* 支持规则对 TCP / UDP 分别管控
* 支持 Network 规则，支持匹配网络类型 ( TCP / UDP )
* 支持逻辑判断规则 ( NOT / OR / AND )
* 支持子规则集
* 支持所有规则的源 IPCIDR 条件，只需附加到末尾即可
* 支持 GEODATA MODE 切换，mmdb / dat
* 支持切换 GEODATA LOADER 模式切换 , 普通 / 小内存模式
* 支持 GeoSite 延迟加载 （无 Geosite 规则，getsite.dat 不下载）
* GEOIP / GEOSITE 数据库基于 Loyalsoldier/v2ray-rules-dat

#### DNS 模块

* 支持 Sniffer 域名嗅探器
* 支持 Fallback-Filter 使用 Geosite
* 恢复 Redir-Host 远程解析
* 支持使用代理解析 ip
* 支持使用 HTTP/3
* 支持 DNS over QUIC

#### TUN 模块

* 支持 macOS、Linux 和 Windows
* 内置 iptables, 无需手动配置
* 内置 Wintun 驱动程序
* 支持 "gvisor / system / lwip" 堆栈

TG 发布频道：[https://t.me/ClashMeta](https://t.me/ClashMeta)  
TG 讨论组：[https://t.me/MetaKernel](https://t.me/MetaKernel)
