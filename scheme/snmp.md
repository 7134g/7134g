#### doc
[官方](https://www.snmp.com/snmpv3/)
[csdn](https://blog.csdn.net/m0_52165864/article/details/127403056)


### go相关库
[prometheus所使用](https://github.com/prometheus/snmp_exporter)
[gosnmp](https://github.com/gosnmp/gosnmp)


### 简述
SNMP是简单网络管理协议（Simple Network Management Protocol）的缩写，
它是一种用于网络设备（如路由器、交换机、服务器等）监控和管理的协议。
SNMP协议定义了一组消息格式，以便管理站点能够远程查询网络设备的状态、配置和性能信息。
SNMP协议通常分为三个部分：管理站点（或者叫管理者）、代理设备（或者叫代理）和网络设备。
管理站点通过发送请求消息来获取网络设备的信息，代理设备则接收这些请求并回应相应的响应消息，
而网络设备则对这些请求做出响应。
SNMP协议被广泛地应用于大型企业和互联网服务提供商中，以帮助管理员监控和管理他们的网络设备。


### 结构


#### SNMP消息包括两个部分：一个头部和一个数据部分。
| IP header | UDP header | version | community | PDU-type | request-id | error-status | error-index | variable bindings |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| IP头 | UDP头 | 版本 | 社区 | PDU型 | 请求ID | 错误状态 | 错误指数 | 变量绑定 |

- IP header：SNMP是基于IP协议的，因此它需要使用IP头部来传递SNMP消息。IP头部中包含了源IP地址和目标IP地址，用于将SNMP消息从一个设备传输到另一个设备。
- UDP header：由于SNMP是基于UDP协议的，因此它需要使用UDP头部来传输SNMP消息。UDP头部中包含了源端口和目标端口，这些端口用于将SNMP消息从一个应用程序传输到另一个应用程序。
- Version：版本字段指定了SNMP协议的版本号，它可以是1、2c或3。不同的版本有不同的特性和功能。
- Community：community字段是一个字符串，用于验证SNMP消息的来源和权限。它包含了两个部分：读取community和写入community。读取community用于获取MIB对象的值，而写入community则用于设置MIB对象的值。只有具有正确的community权限的请求才会被SNMP代理响应。
- PDU-type：PDU（Protocol Data Unit）类型字段指定了SNMP消息的类型，例如GetRequest、SetRequest、GetNextRequest等。
- Request-id：request-id字段是一个整数，用于识别SNMP消息的唯一性。当SNMP代理响应SNMP请求时，它将使用相同的request-id字段，以便发起者可以识别哪个SNMP请求收到了响应。
- Error-status：error-status字段是一个整数，用于指示SNMP请求操作的结果。如果SNMP请求成功，则该字段为0。否则，它将包含一个错误代码，例如noSuchName、readOnly等。
- Error-index：error-index字段是一个整数，用于指示SNMP请求操作失败时引起错误的MIB对象的索引。例如，在GetBulkRequest中，如果其中一个OID的值无法被获取，则error-index字段将指示哪个OID引起了错误。
- Variable bindings：variable bindings字段包含了一组MIB对象和它们的值。在GetRequest、SetRequest、GetNextRequest和GetBulkRequest消息中，variable bindings字段用于指定操作所涉及的MIB对象。在Response消息中，variable bindings字段包含了请求的结果。

其中，variable bindings 是数据部分，其余都是头部

#### Variable binding 结构
- Object Identifier（OID）：OID是一个由数字和点号（.）连接起来的序列，用于唯一地标识MIB树中的每个节点。在Variable binding中，OID字段指定了要获取或设置的MIB对象的位置。
- Value：值字段则是具体的数据类型和值，表示相应的MIB对象的当前值。它可以是整数、字符串、布尔值、空值等各种数据类型，具体的数据类型由SNMP消息头部中的PDU-type字段决定。

例子：
```yaml
Variable bindings:
   - Object identifier: 1.3.6.1.2.1.1.1.0
     Value: Octet string, length = 47
            "Cisco IOS Software, C3560CX Software ("
   - Object identifier: 1.3.6.1.2.1.1.4.0
     Value: Octet string, length = 16
            "admin@example.com"
```
在此示例中，GetRequest消息包含了两个Variable bindings，分别用于获取设备的系统信息和联系人信息。第一个Variable binding的OID为1.3.6.1.2.1.1.1.0，它标识了设备的sysDescr。该MIB对象的值为字符串类型，其值为"Cisco IOS Software, C3560CX Software ("。第二个Variable binding的OID为1.3.6.1.2.1.1.4.0，它标识了设备的sysContact。该MIB对象的值也为字符串类型，其值为"admin@example.com"。

这些值可以通过SNMP代理从设备上获取，并以此生成相应的GetResponse消息发送回管理系统。在GetResponse消息中，Variable bindings字段将包含请求的结果，即获取到的MIB对象及其对应的值。

#### SNMP PDU

snmp七种请求类型：
- GetRequest: 代表一个请求，用于获取单个或多个MIB对象的值。
- SetRequest: 代表一个请求，用于设置单个或多个MIB对象的值。
- GetNextRequest: 代表一个请求，用于获取比给定OID大的下一个OID所对应的MIB对象的值。
- GetBulkRequest: 代表一个请求，用于获取大量MIB对象的值。与GetNextRequest类似，但可发出一次多个OID的查询。
- Response: 代表一个响应，包含了GetRequest、SetRequest、GetNextRequest、GetBulkRequest中请求的结果。
- Trap: 代表一个通知消息，用于向管理者报告发生了重要事件，例如某个设备已经宕机或某个端口已经关闭等。
- InformRequest: 代表一种确认消息，它允许管理者收到Trap消息后确认其接收成功。
							


snmp包的GetRequest请求例子：
```yaml
{
  "ip-header": {...},
  "udp-header": {...},
  "version": 3,
  "msg-flags": "authPriv",
  "security-model": 3,
  "security-name": "myuser",
  "context-engine-id": "1234567890",
  "context-name": "mycontext",
  "pdu-type": "get-request",
  "request-id": 54321,
  "error-status": 0,
  "error-index": 0,
  "variable-bindings": [
    {
      "name": "sysDescr.0",
      "value": null
    },
    {
      "name": "sysUpTime.0",
      "value": null
    }
  ],
  "authentication-parameters": "<hashed-authentication-password>",
  "privacy-parameters": "<encrypted-privacy-password>"
}


IP header 和 UDP header 是标准的网络层和传输层协议头部。
version 值为3，表示使用SNMPv3版本。
msg-flags 表示消息标志位，在本例中值为"authPriv"，表示进行了认证（authentication）和加密（privacy）。
security-model 值为3，表示使用 USM 安全模型。
security-name 字段指定了用于身份验证和授权的用户名称.
context-engine-id 和 context-name 指定了请求的上下文。
pdu-type 值为"get-request"，表示这是一个SNMP GET 请求。
request-id 是请求标识符，SNMP代理应该将其包含在响应中以便识别此操作的唯一性。
error-status 和 error-index 字段用于响应错误信息。
variable-bindings 列出了要获取的变量列表。在这里我们请求了 "sysDescr.0" 和 "sysUpTime.0" 这两个系统描述变量。
authentication-parameters 和 privacy-parameters 用于存储经过哈希和加密处理后的密码，以保证通信过程的安全性。
```

