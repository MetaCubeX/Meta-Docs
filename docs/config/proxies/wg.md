# wg

```
  - name: "wg"
    type: wireguard
    server: 162.159.192.1
    port: 2480
    ip: 172.16.0.2
    ipv6: fd01:5ca1:ab1e:80fa:ab85:6eea:213f:f4a5
    public-key: Cr8hWlKvtDt7nrvf+f0brNQQzabAqrjfBvas9pmowjo=
    #    pre-shared-key: 31aIhAPwktDGpH4JDhA8GNvjFXEf/a6+UaQRyOAiyfM=
    private-key: eCtXsJZ27+4PbhDkHnB923tkUn2Gj59wZw5wFA75MnU=
    udp: true
    reserved: "U4An"
    # 数组格式也是合法的
    # reserved: [209,98,59]
    # 如果peers不为空,该段落中的allowed_ips不可为空；前面段落的server,port,ip,ipv6,public-key,pre-shared-key均会被忽略,但private-key会被保留且只能在顶层指定
    # peers:
    #   - server: 162.159.192.1
    #     port: 2480
    #     ip: 172.16.0.2
    #     ipv6: fd01:5ca1:ab1e:80fa:ab85:6eea:213f:f4a5
    #     public-key: Cr8hWlKvtDt7nrvf+f0brNQQzabAqrjfBvas9pmowjo=
    #     # pre-shared-key: 31aIhAPwktDGpH4JDhA8GNvjFXEf/a6+UaQRyOAiyfM=
    #     allowed_ips: ['0.0.0.0/0']
    #     reserved: [209,98,59]
```
