---
hide:
  - navigation
#   - toc
---
# API说明

## API密钥

请求时附带  `'Authorization: Bearer secret'`  请求头,`secret` 为配置文件设置的api密钥

curl示例 `curl -H 'Authorization: Bearer secret' 127.0.0.1:9090/version`

## 日志

#### `/logs`

请求方法:  `GET`

* 获取实时日志

## 流量信息

#### `/traffic`

请求方法:  `GET`

* 获取实时流量,单位 kbps

## 内存信息

#### `/memory`

请求方法:  `GET`

* 获取实时内存占用,单位 kb

## 版本信息

#### `/version`

请求方法:  `GET`

* 获取clash版本

## 缓存

#### `/cache/fakeip/flush`

#### 请求方法:  `POST`

* 清除fakeip缓存

## 运行配置

#### `/configs`

请求方法:  `GET`

* 获取基本配置

请求方法:  `PUT`

* 重新加载基本配置
* URL需携带 `?force=true` 强制执行,必须发送数据
* curl示例: `curl "127.0.0.1:9090/configs?force=true" -X PUT -d '{"path": "", "payload": ""}'`

请求方法:  `PATCH`

* 更新基本配置,传入需要修改的配置即可,传入的数据需以json格式传入
* 示例: `curl 127.0.0.1:9090/configs -X PATCH -d '{"mixed-port": 7890}'`

#### `/configs/geo`

#### 请求方法:  `POST`

* 更新GEO数据库
* 必须发送数据,因更新后会自动重载一次配置
* curl示例: `curl "127.0.0.1:9090/configs" -X POST -d '{"path": "", "payload": ""}'`

#### `/restart`

#### 请求方法:  `POST`

* 重启内核
* 必须发送数据
* curl示例: `curl "127.0.0.1:9090/restart " -X POST -d '{"path": "", "payload": ""}'`

#### `/upgrade`

#### 请求方法:  `POST`

* 更新内核
* 必须发送数据,因更新后会自动重载一次配置
* curl示例: `curl "127.0.0.1:9090/upgrade" -X POST -d '{"path": "", "payload": ""}'`

## 代理

#### `/proxies`

请求方法:  `GET`

* 获取代理信息

#### `/proxies/:name`

请求方法:  `GET`

* 获取具体的代理信息

请求方法:  `PUT`

* 选择特定的代理

#### `/proxies/:name/delay`

请求方法:  `GET`

* 获取具体代理的延迟测试信息

## 规则

#### `/rules`

请求方法:  `GET`

* 获取规则信息

## 连接

#### `/connections`

请求方法:  `GET`

* 获取连接信息

请求方法:  `DELETE`

* 关闭所有连接

#### `/connections/:id`

请求方法:  `DELETE`

* 关闭特定连接

## 代理集合

#### `/providers/proxies`

请求方法:  `GET`

* 获取所有代理集合的所有代理信息

#### `/providers/proxies/:name`

请求方法:  `GET`

* 获取特定代理集合的代理信息

请求方法:  `PUT`

* 更新的代理集合

#### `/providers/proxies/:name/healthcheck`

请求方法:  `GET`

* 触发特定代理集合的健康检查

## 规则集合

#### `/providers/rules`

#### `/providers/rules/:name`

## 域名查询

#### `/dns/query`

请求方法:  `GET`

* 获取指定名称和类型的 DNS 查询数据

参数

* `name`（必填）：要查询的域名。
* `type`（可选）：要查询的 DNS 记录类型(例如，A、MX、CNAME 等)

示例: `GET /dns/query?name=example.com&type=A`
