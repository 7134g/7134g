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
							



#### 其他
安全模型（Security Model）：用于指定SNMP消息的安全模型，常用的安全模型有SNMPv1/v2c兼容模型、
用户安全模型和命令行安全模型等。

上下文引擎ID（Context Engine ID）：用于标识一个具体的SNMP引擎实例，通常使用IP地址或MAC地址等唯一标识。

上下文名称（Context Name）：用于标识管理数据的上下文，例如网络中的不同子网、不同虚拟专有网络（VLAN）、不同的路由器接口等。

填充（Padding）：用于保证消息长度为8的倍数，以便于进行加密和解密。

权威引擎ID（Authoritative Engine ID）：用于标识消息的发送者，通常是SNMP代理或管理器的引擎ID。

认证参数（Authentication Parameters）：用于提供对报文进行认证的必要信息，通常使用散列函数计算出的加密校验值。

加密参数（Privacy Parameters）：用于提供对报文进行加密的必要信息，通常使用随机数生成的密钥和初始化向量等。

引擎启动计数器（Engine Boots）：用于指示SNMP引擎自身启动的次数，以及SNMP引擎重启后所产生的引擎时间。

PDU数据（PDU Data）：用于存储SNMP请求或响应的具体内容，例如要获取或修改的变量OID值、变量类型、变量值等。

消息认证码（Message Authentication Code）：用于确保消息在传输过程中没有被篡改或伪造，
通常使用散列函数计算出的MAC值来实现。

私有协议数据单元（Private Protocol Data Unit）：用于提供对PDU进行加密的必要信息，具体内容包括密钥和初始化向量等。

加密（Encryption）：用于对整个SNMPv3报文进行加密处理，以保护其中的敏感信息不被窃听和泄露。






有三种主要的信息类型：

- Get：用于请求获取一个或多个 OID 的值。
- Set：用于修改一个或多个 OID 的值。
- Trap：用于向管理系统发送事件通知，例如系统故障或警告。

OID 是一种唯一标识网络对象的方式。每个 OID 都由一系列数字组成，用点号分隔，类似于 IP 地址。
例如，OID 1.3.6.1.2.1.1 表示系统信息，OID 1.3.6.1.2.1.2 表示接口信息。

