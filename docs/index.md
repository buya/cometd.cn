前言
================================
CometD是一个可伸缩的web事件路由总线，它允许您编写低延迟、服务器端、事件驱动的web应用程序。此类应用程序的典型示例是股票交易应用程序、web聊天应用程序、在线游戏和监控控制台。

CometD为您提供了实现这些消息模式的API:包括发布/订阅、点对点(通过服务器)和远程过程调用。这是通过使用一个独立的传输协议(Bayeux协议)实现的，它可以通过HTTP或WebSocket(或其他传输协议)进行传输，这样您的应用程序就不会绑定到特定的传输技术。

CometD可以利用WebSocket(因为它是最高效的web消息传递协议)，而在使用HTTP时使用了被称为Comet的Ajax推送技术模式。

CometD项目提供了Java和JavaScript库，允许您以简单和可移植的方式编写低延迟、服务器端、事件驱动的web应用程序。因此，您可以将精力集中在应用程序的业务方面，而不必担心诸如传输(HTTP或WebSocket)、可伸缩性和健壮性等底层细节。CometD库已经提供了这些的特性。

3.安装
================================
3.1.要求与依赖
================================
为了运行CometD应用程序，您需要Java开发工具包(JDK)-version 7.0或更高版本，以及一个兼容的Servlet 3.0或更高的Servlet容器，如Jetty。

CometD的实现依赖于很少的Jetty库，比如Jetty-util-ajax-<version>.jar及其他。这些Jetty依赖通常被打包在应用程序的WEB-INF/lib目录中的.war文件中，不需要部署应用程序。Jetty中的war文件:基于cometd的应用程序将在任何其他兼容的Servlet 3.0或更高的Servlet容器中以同样的方式工作。

当前的Jetty版本CometD依赖默认为:

	<jetty-version>9.2.22.v20170606</jetty-version>
	<!--<jetty-version>9.3.21.v20170918</jetty-version>-->
	<!--<jetty-version>9.4.7.v20170914</jetty-version>-->

从Jetty 9.2.x开始，可以使用不同的Jetty版本来运行CometD 。您可以找到如何使用不同的Jetty版本构建CometD的示例，如何使用不同的Jetty版本运行CometD演示，以及如何使用不同的Jetty版本创建web应用程序。

3.2.下载与安装
================================
你可以从[http://download.cometd.org/](http://download.cometd.org/)下载CometD。

然后在您选择的目录中解压CometD:

	$ tar zxvf cometd-<version>-distribution.tgz
	$ cd cometd-<version>/

3.3.运行DEMOS
================================
CometD Demos包括：

- 三个完整的聊天应用程序(一个是与Dojo一起开发的，一个是jQuery的，一个是Angular1)；
- 包括：消息确认、重载、timesync和时间戳等的示例；
- 一个如何向特定用户发送私有消息的示例；
- 集群拍卖演示(使用Oort集群)。


6.概念和架构
================================
CometD 工程实现了多种 Comet 技术 来提供可伸缩的web消息系统，它可以通过HTTP或者其他各种web协议，比如websocket

6.1 定义
----------------------
 客户端初始化连接，服务器接受连接，建立长连接，就是一直打开除非任意一方关闭它

 典型的客户端是浏览器，但是也可以是其他的应用，比如java应用，浏览器插件，或者其他脚本语言。

 根据使用的comet技术不同，一个客户端可能和服务器打开一个或者多个物理连接，但是可以认为在服务器和客户端之间只有一个逻辑的管道

 CometD 工程使用了 Bayeux 协议来交换服务器与客户端之间的信息，通信的单元是一个JSON格式的Bayeux 消息，一个消息包含了几个字段，一些是由Bayeux约定的，其他的可能是应用自己定义添加的。

 客户端和服务器交换的所有的消息都有一个 channel 字段， channel 提供了消息的分类， channel 是 CometD 的核心概念：发布者发送一个消息到 channel，channel的订阅者然后到了消息。

 6.1.1.Channel 定义
--------------------------------------
 一个 channnel 是一个看起来像 URL 的字符串,比如 /foo/bar ,/meta/connect ,/service/chat .

Bayeux 标准定义了三种 channel ：mata channels,service channels和broadcast channels。

以 /meta/ 开始的channel 是一个 meta channel，以 /service/ 开始的是 service channel ,其他的都是 broadcast channel.

channel字段是meta channel 的消息就是一个 meta 消息，service 消息和 broadcast 消息也一样。

应用可以任意创建 service channel 和 broadcast channel.

### 6.1.1.1.Meta Channels

Cometd 会创建 meta channels;应用不能创建新的 mata channels ，Meta channels 主要用来和应用交互 Bayeux 协议相关的一些信息，比如，握手有没有成功，或者连接被断开或者重连。

### 6.1.1.2 Service Channels

应用创建 service channels 用来满足服务器和客户端之间 request/response 风格的通信 （和 publish/subsribe 的风格相对应）。

### 6.1.1.3 Broadcast Channels

应用也可以创建 broadcase channels,它有消息主题的语义,被用于 publish/subsribe 风格的通讯,比如一个发送者想把消息广播给多个接收者

# 6.2 抽象定义

Cometd 实现了 web 消息系统，特别是 publish/subcribe 的场景.

在 publish/subcribe 消息系统里面，发布者发送分好类的消息，订阅者订阅一种或者多种消息，这样他们仅仅接受他们订阅的感兴趣消息，消息的发送这，是不知道他们发送的消息有多少人接收的。

CometD是一个 hub-spoke 拓扑，在默认的配置里，就意味这有一个中心服务器，所有的客户端都连接上面

![](images/hub_spoke.png)


在 CometD 里面，服务器接受发布者的消息，如果消息的channel 是 broadcast 的话





