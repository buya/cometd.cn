前言
================================
CometD是一个可伸缩的web事件路由总线，它允许您编写低延迟、服务器端、事件驱动的web应用程序。此类应用程序的典型示例是股票交易应用程序、web聊天应用程序、在线游戏和监控控制台。

CometD为您提供了实现这些消息模式的API:包括发布/订阅、点对点(通过服务器)和远程过程调用。这是通过使用一个独立的传输协议(Bayeux协议)实现的，它可以通过HTTP或WebSocket(或其他传输协议)进行传输，这样您的应用程序就不会绑定到特定的传输技术。

CometD可以利用WebSocket(因为它是最高效的web消息传递协议)，而在使用HTTP时使用了被称为Comet的Ajax推送技术模式。

CometD项目提供了Java和JavaScript库，允许您以简单和可移植的方式编写低延迟、服务器端、事件驱动的web应用程序。因此，您可以将精力集中在应用程序的业务方面，而不必担心诸如传输(HTTP或WebSocket)、可伸缩性和健壮性等底层细节。CometD库已经提供了这些的特性。

3.安装
================================
### 3.1.要求与依赖
为了运行CometD应用程序，您需要Java开发工具包(JDK)-version 7.0或更高版本，以及一个兼容的Servlet 3.0或更高的Servlet容器，如Jetty。

CometD的实现依赖于很少的Jetty库，比如Jetty-util-ajax-<version>.jar及其他。这些Jetty依赖通常被打包在应用程序的WEB-INF/lib目录中的.war文件中，不需要部署应用程序。Jetty中的war文件:基于cometd的应用程序将在任何其他兼容的Servlet 3.0或更高的Servlet容器中以同样的方式工作。

当前的Jetty版本CometD依赖默认为:

	<jetty-version>9.2.22.v20170606</jetty-version>
	<!--<jetty-version>9.3.21.v20170918</jetty-version>-->
	<!--<jetty-version>9.4.7.v20170914</jetty-version>-->

从Jetty 9.2.x开始，可以使用不同的Jetty版本来运行CometD 。您可以找到如何使用不同的Jetty版本构建CometD的示例，如何使用不同的Jetty版本运行CometD演示，以及如何使用不同的Jetty版本创建web应用程序。

### 3.2.下载与安装
你可以从[http://download.cometd.org/](http://download.cometd.org/)下载CometD。

然后在您选择的目录中解压CometD:

	$ tar zxvf cometd-<version>-distribution.tgz
	$ cd cometd-<version>/

### 3.3.运行DEMOS
CometD Demos包括：

- 三个完整的聊天应用程序(一个是与Dojo一起开发的，一个是jQuery的，一个是Angular1)；
- 包括：消息确认、重载、timesync和时间戳等的示例；
- 一个如何向特定用户发送私有消息的示例；
- 集群拍卖演示(使用Oort集群)。

##### 3.3.1.使用MAVEN运行DEMOS
如果你想看看CometD演示/或者想尝试您的应用程序，建议以这种模式的运行CometD演示,但它不是推荐的方式来部署一个CometD在生产中的应用。请参阅下一节，了解如何在生产中部署CometD应用程序。

Maven要求您设置JAVA_HOME环境变量以指向您的JDK安装。

在此之后，运行CometD演示非常简单。假设$COMETD是COMETD安装目录，并且在您的PATH中有mvn可执行文件:

	$ cd $COMETD
	$ cd cometd-demo
	$ mvn jetty:run

最后用命令启动嵌入式Jetty来监听8080端口。现在将浏览器指向http://localhost:8080，查看CometD演示主页。

如果您想用不同的Jetty版本运行CometD演示，请将最后一个命令更改为:

	$ mvn jetty:run -Djetty-version=<alternate-jetty-version>

例如：

	$ mvn jetty:run -Djetty-version=9.3.10.v20160621


----------
 - 注意：使用Jetty9.2.x运行CometD要求JDK7版本，使用Jetty9.3.x或更高版本要求至少JDK8.

### 3.4.部署CometD应用程序
当您开发CometD应用程序时，您将开发一个标准的Java EE Web应用程序，然后将其打包成一个war文件。您可以遵循入门部分或CometD教程，以了解如何构建和打包CometD应用程序。

一旦您将CometD应用程序打包为一个war文件，您可以将其部署到任何支持Servlet 3.0或更高版本的Servlet容器中。

请参阅本节以获取进一步的信息，以及有关在Servlet 3.0(或更大)容器上部署的具体说明。

##### 3.4.1.在独立的Jetty中部署CometD应用程序
下面的说明描述了运行CometD应用程序所需的非常小的Jetty设置。有关配置Jetty的详细信息，请参阅官方Jetty文档。

按照以下步骤将CometD应用程序部署到Jetty中。这些指令对于Unix/Linux操作系统是有效的，但是可以很容易地翻译为Windows操作系统。

从Eclipse Jetty下载Jetty发行版。

然后在您选择的目录中解压Jetty发行包，例如/tmp:

	$ cd /tmp
	$ tar zxvf jetty-distribution-<version>.tar.gz
这就创建了一个的名为/tmp/jetty-distribution -<版本>/的目录，它被作为JETTY_HOME。

创建您选择的另一个目录，例如在您的主目录中:

	$ cd ~
	$ mkdir jetty_cometd

这将创建一个名为~/jetty_cometd的目录，该目录作为JETTY_BASE。

由于Jetty是一个高度模块化的Servlet容器，JETTY_BASE配置为您的Jetty目录，其中包含运行CometD应用程序所需的Jetty模块。

为了运行CometD应用程序，Jetty需要配置一些模块:

- http模块，为http协议提供支持。
- websocket模块，为websocket协议提供支持。
- 部署模块，为部署.war文件提供支持 

因此：

	$ cd $JETTY_BASE
	$ java -jar $JETTY_HOME/start.jar --add-to-start=http,websocket,deploy

现在Jetty被配置为运行CometD应用程序，而您只需要部署您的.war文件到Jetty(如果您还没有构建您的应用程序可以使用CometD演示.war文件)：

	$ cp /path/to/cometd_application.war $JETTY_BASE/webapps/

现在可以启动Jetty:

	$ cd $JETTY_BASE
	$ java -jar $JETTY_HOME/start.jar

最后用一个命令启动Jetty，它部署您的.war文件，并激活您的CometD应用程序。

##### 3.4.2.使用嵌入式Jetty部署CometD应用程序
您还可以通过编程方式部署CometD应用程序，使用Jetty api将您的web应用程序嵌入到您将从命令行开始的普通Java应用程序中。

下面你可以找到一个{github}/cometd-demo/src/test/java/org/cometd/demo/Demo.java[示例]展示了如何建立支持HTTP、HTTPS和WebSocket传输的嵌入式Jetty和CometD:

	// Setup and configure the thread pool.
	QueuedThreadPool threadPool = new QueuedThreadPool();
	
	// The Jetty Server instance.
	Server server = new Server(threadPool);
	
	// Setup and configure a connector for clear-text http:// and ws://.
	HttpConfiguration httpConfig = new HttpConfiguration();
	ServerConnector connector = new ServerConnector(server, new HttpConnectionFactory(httpConfig));
	connector.setPort(HTTP_PORT);
	server.addConnector(connector);
	
	// Setup and configure a connector for https:// and wss://.
	SslContextFactory sslContextFactory = new SslContextFactory();
	sslContextFactory.setKeyStorePath("src/test/resources/keystore.jks");
	sslContextFactory.setKeyStorePassword("storepwd");
	sslContextFactory.setKeyManagerPassword("keypwd");
	HttpConfiguration httpsConfig = new HttpConfiguration(httpConfig);
	httpsConfig.addCustomizer(new SecureRequestCustomizer());
	ServerConnector tlsConnector = new ServerConnector(server, sslContextFactory, new HttpConnectionFactory(httpsConfig));
	tlsConnector.setPort(8443);
	server.addConnector(tlsConnector);
	
	// The context where the application is deployed.
	ServletContextHandler context = new ServletContextHandler(server, CONTEXT_PATH);
	
	// Configure WebSocket for the context.
	WebSocketServerContainerInitializer.configureContext(context);
	
	// Setup JMX.
	MBeanContainer mbeanContainer = new MBeanContainer(ManagementFactory.getPlatformMBeanServer());
	server.addBean(mbeanContainer);
	context.setInitParameter(ServletContextHandler.MANAGED_ATTRIBUTES, BayeuxServer.ATTRIBUTE);
	
	// Setup the default servlet to serve static files.
	context.addServlet(DefaultServlet.class, "/");
	
	// Setup the CometD servlet.
	String cometdURLMapping = "/cometd/*";
	ServletHolder cometdServletHolder = new ServletHolder(CometDServlet.class);
	context.addServlet(cometdServletHolder, cometdURLMapping);
	// Required parameter for WebSocket transport configuration.
	cometdServletHolder.setInitParameter("ws.cometdURLMapping", cometdURLMapping);
	// Optional parameter for BayeuxServer configuration.
	cometdServletHolder.setInitParameter("timeout", String.valueOf(15000));
	// Start the CometD servlet eagerly to show up in JMX.
	cometdServletHolder.setInitOrder(1);
	
	// Add your own listeners/filters/servlets here.
	
	server.start();



> 如果您的应用程序需要使用WebSocket客户端连接其他服务，您可能会在启动时获得NoSuchMethodError错误。

> 如果是这样的话，那是因为事实上(Maven格式<groupId>:<artifactId>) org. java:cometd-java-websocket-jetty-client组件依赖于Jetty的混合分类器org.eclipse.jetty.websocket:websocket-client。

> 该组件的混合版本用于部署使用war文件的web应用程序，是用于典型的部署CometD web应用程序。

> 在嵌入式模式中，您不能使用该组件的混合版本，而是应使用常规版本。这通常是通过显式地排除混合版本和显式地包含组件的常规版本来完成的。

> 例如，使用Maven:

	<dependencies>
    ...
    <!-- Explicitly exclude the hybrid dependency. -->
    <dependency>
        <groupId>org.cometd.java</groupId>
        <artifactId>cometd-java-websocket-jetty-client</artifactId>
        <version>${cometd.version}</version>
        <exclusions>
            <exclusion>
                <groupId>org.eclipse.jetty.websocket</groupId>
                <artifactId>websocket-client</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <!-- Explicitly include the regular dependency. -->
    <dependency>
        <groupId>org.eclipse.jetty.websocket</groupId>
        <artifactId>websocket-client</artifactId>
        <version>${jetty.version}</version>
    </dependency>
    ...
	</dependencies>

##### 3.4.3.使用另一个Servlet容器运行演示
将CometD应用程序部署到不同的Servlet容器中需要做的是与上面描述的Jetty类似的步骤。

对于您选择的Servlet容器，请参考具体的Servlet容器配置手册，来了解如何部署CometD应用程序.war文件。

4.故障诊断
================================
### 4.1.日志
当CometD不起作用时，您要做的第一件事是启用调试日志记录。这是有帮助的，因为通过阅读调试日志，您可以更好地了解系统中正在发生的事情(并且仅这一点就可以为您提供解决问题所需的答案)，并且CometD开发人员可能需要调试日志来帮助您。

##### 4.1.1.启用JavaScript库中的调试日志
要在JavaScript客户端库中启用调试日志(请参阅JavaScript库部分)，您必须在cometd的配置对象中配置logLevel字段为“debug”值(请参阅JavaScript库配置部分的其他配置选项)：

		cometd.configure({
		    url: 'http://localhost:8080/cometd',
		    logLevel: 'debug'
		});

CometD JavaScript库使用window控制台对象输出日志语句。

一旦您在JavaScript客户端库中启用了日志记录，您就可以在浏览器JavaScript控制台中看到调试日志。例如,在Firefox中您可以通过点击工具→Web开发人员→Web控制台来打开JavaScript控制台,在Chrome可以通过点击工具→开发工具打开JavaScript控制台。

> 注意：Internet Explorer没有和其他浏览器一样定义window控制台对象，导致CometD不能在JavaScript控制台上输出日志，这取决于Internet Explorer版本。

##### 4.1.2.在Java服务器库中启用调试日志记录
CometD Java库(客户机和服务器)都使用SLF4J作为日志框架。Java服务器库使用SLF4J api，但是它不绑定到任何特定的日志实现。您必须选择要使用的实现。默认情况下，SLF4J附带了一个简单的绑定，它不记录在调试级别生成的语句。

因此，您必须使用更高级的绑定(如Log4J或Logback)配置SLF4J。了解如何使用高级绑配置日志请参考SLF4J文档和绑定实现文档。

因此，为您的应用程序启用CometD调试日志，您需要配置您选择使用的任何SLF4J绑定。

一个简单的例子是，对于Log4J，通常可以在WEB-INF/lib中添加SLF4J-Log4J绑定jar 包(slf4j-log4j12-<slf4j-version>.jar)和Log4J jar (Log4J-<Log4J-version>.jar)，然后在web-inf/classes目录下添加一个适当的log4j.properties属性配置文件。您当然不希望手动添加这些文件;这里只是作为一个简单的参考。通常，这些依赖关系配置在您用来创建web应用程序的构建系统中。

一旦您在Java服务器库中启用了日志记录，您就可以在启动了Servlet容器的终端中看到调试日志，您在其中(或者在其日志文件中，这取决于它的配置方式)。

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





