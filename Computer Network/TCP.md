#### TCP

- 面向连接的、可靠的、基于字节流的传输层通信协议

- [ ] 特点

- 基于连接的:数据传输之前需要建立连接
- 全双工:双向传输
- 字节流:不限制数据大小,打包成报文段,保证有序接收,重复报文自动丢弃
- 流量缓冲:解决双方处理能力的不匹配
- 可靠的传输服务:保证可达,丢包时可以通过重发机制实现可靠性
- 拥塞控制:防止网络出现恶行拥塞



#### TCP连接管理

**先举个例子**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200709200540124.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2phY2tfd2FuZzEyODgwMQ==,size_16,color_FFFFFF,t_70)

#### 

- [ ] 1.TCP连接:四元组[源地址、源端口、目的地址、目的端口]

> 与web服务器连接的一般是80端口

- [ ] 2.确定连接:TCP三次握手

- 三次握手的目的

- [ ] 用处
- 1.确定双方的交付能力
- 2.指定初始序列号(==随机产生==),为可靠传输做准备
- 3.交互保密机制
-  [ ] 握手过程(粗略的说法)
- 1.客户端给服务器发送一个SYN报文(==无数据部分==)
- 2.服务器收到SYN报文后,会应答一个SYN+ACK报文(==无应用层数据==)
- 3.客户端收到SYN+ACK报文后,回应一个ACK报文(==可携带数据==)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200630152614697.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2phY2tfd2FuZzEyODgwMQ==,size_16,color_FFFFFF,t_70)

- [ ] 握手过程(精确的说法)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200630220624801.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2phY2tfd2FuZzEyODgwMQ==,size_16,color_FFFFFF,t_70)
- 1.客户端发送SYN报文,指定序列号ISN,处于SYN_Send状态
- 2.服务端把SYN作为应答,指定ISN,把客户端的ISN+1作为ACK值发送给客户端,此时服务器处于SYN_RECV状态
- 3.客户端发送一个ACK报文,此时客户端处于established状态
- 4.服务器收到ACK报文之后也处于established状态

补充:
- [ ] 半连接队列:服务器第一次收到客户端的SYN之后,就会处于SYN_RCVD状态,此时双方还没有完全建立其连接,服务器回把这种状态下请求的连接放入一个队列,我们把这个队列称为**半连接队列**(用于保存处于SYN_SEND 和 SYN_RECV状态的请求)
- [ ] **全连接队列**:已经完成三次握手,建立起连接的就会放到全连接队列中,如果队列满了就会出现丢包的可能性.

##### SYN洪泛攻击(针对三次握手)
SYN洪泛攻击发生在OSI第四层,这种方式利用TCP协议的三次握手,攻击者发送TCP SYN,而服务器返回 ACK之后,攻击者就对其进行再次确认,那这个TCP连接就处于挂起状态,也就是所谓的半连接状态,服务器收不到在确认的话,还会重复发送给ACK给攻击者。这样更加会浪费服务器的资源,攻击者就对服务器发送非常大量的这种TCP连接,由于每一个都没完成三次握手,所以在服务器上,这些TCP连接会因为挂起状态而消耗CPU和内存,最后服务器可能死机,就无法为正常用户提供服务了.

##### 解决办法(SYN cookie)
SYN Cookie是对TCP服务器端的三次握手协议作一些修改，专门用来防范SYN Flood攻击的一种手段。它的原理是，在TCP服务器收到TCP SYN包并返回TCP SYN+ACK包时，不分配一个专门的数据区，而是根据这个SYN包计算出一个cookie值。在收到TCP ACK包时，TCP服务器在根据那个cookie值检查这个TCP ACK包的合法性。如果合法，再分配专门的数据区进行处理未来的TCP连接。



### 查看连接状态

- netstat -ntp -c 1

> -c n 表示间隔时间,每间隔n秒钟去查看一次
>
> - -t :表示TCP
> - -p:表示打印出对应的IP
> - -n:表示用数字形式显示

- netstat -nltp







#### TCP四次挥手



等待2MSL的原因

- 1.防止报文丢失,导致被动端重复发送FIN
- 2.防止滞留在网络中的报文,对新建立的连接造成数据扰乱





### 重传机制

1.ack报文丢失,超时重传该报文

2.请求报文丢失,超时重传该报文





#### 滑动窗口

大小通过tcp三次握手和对端协商,且受到网络状况影响



