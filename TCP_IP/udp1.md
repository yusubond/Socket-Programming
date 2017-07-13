## UDP:用户数据报协议

UDP是一个简单的面向数据包的运输层协议：进程的每个输出操作都正好产生一个UDP数据报，并组装成一份待发送的IP数据报。

![udp-package](http://on64c9tla.bkt.clouddn.com/Comput/udp-package.png)

UDP不提供可靠性：它把应用程序传给IP层的数据发送出去，但是并不保证它们能到达目的地。

### UDP首部

![udp-head](http://on64c9tla.bkt.clouddn.com/Comput/udp-head.png)

+ 端口号表示发送进程和接收进程。因为IP层已经把IP数据报分配给TCP或UDP(ip首部协议字段值)，所以TCP端口由TCP来查看，UDP端口由UDP来查看。**TCP端口号和UDP端口号是相互独立的**。**但是，如果TCP和UDP同时提供某种服务，两个协议通常选择相同的端口号**。

+ UDP长度字段指UDP首部和UDP数据的字节长度。最小值是8字节(即，0字节的UDP数据报)。

+ UDP检验和覆盖UDP首部和UDP数据。**而，IP首部中，只覆盖IP的首部，并不覆盖IP数据报中的任何数据**。

TCP段和UDP数据报都包含一个12字节长度的伪首部。(为了计算校验和而设置的)

![udp-head2](http://on64c9tla.bkt.clouddn.com/Comput/udp-head2.png)

UDP校验和是一个端到端的校验和，由发送端计算，接收端验证。其目的是为了发现UDP首部和数据在发送端到接收端之间发生任何改动。

### IP分片

物理网络层一般要限制每次发送数据帧的最大长度。*任何时候IP层收到一份需要发送的IP数据报时，它要判断向本地哪个接口发送数据(选路)，并查询该接口获得其MTU。IP把MTU与数据报长度进行比较，如果需要则进行分片。分片可以发生在原始发送端主机，也可以发生在中间路由器上*。

把一份IP数据报分片后，只有到达目的地才进行重新组装(要求在下一站就进行重新组装，而不是最终的目的地)。重新组装由目的端的IP层来完成，其目的是使分片和重新组装过程对运输层(TCP和UDP)是透明的。

![udp-slice](http://on64c9tla.bkt.clouddn.com/Comput/udp-slice.png)

注意：**IP数据报是指IP层端到端的传输单元(在分片之前和重新组装之后)，分组是指在IP层和链路层之间传送的数据单元。一个分组可以是一个完整的IP数据报，也可以是IP数据报的一个分片**。

*发生ICMP不可达差错的一个情况是，当路由器收到一份需要分片的数据报，而在IP首部又设置了不分片的标志比特。*