# API说明

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

## 运行配置

#### `/configs`

请求方法:  `GET`

* 获取基本配置

请求方法:  `PUT`

* 重新加载基本配置

请求方法:  `PATCH`

* 更新基本配置

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

* 选择特定的代理集合

#### `/providers/proxies/:name/healthcheck`

请求方法:  `GET`

* 触发特定代理集合的健康检查

## API密钥

请求时附带  `'Authorization: Bearer secret'`  请求头,`secret` 为配置文件设置的api密钥
