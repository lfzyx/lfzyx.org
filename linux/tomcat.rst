Tomcat - java 容器
====================

Tomcat即是一个Http服务器也是一个Servlet容器

安装
----

::

 apt-get install default-jdk
 wget http://www.us.apache.org/dist/tomcat/tomcat-8/v8.0.9/bin/apache-tomcat-8.0.9.tar.gz
 tar zxvf apache-tomcat-8.0.9.tar.gz
 cd apache-tomcat-8.0.9
 bin/startup.sh

结构 [#]_
---------

bin
~~~

存放启动和关闭tomcat脚本

daemon.sh
^^^^^^^^^

setenv.sh
^^^^^^^^^

CATALINA_OPTS="$CATALINA_OPTS -server -Xms600m -Xmx600m -Xmn200m
-XX:userParNewGC -XX:ParallelGCThreads -XX:UseParallelGC
-Djava.awt.headless=true -Xloggc:/tmp/gc.logs"

CATALINA_OPTS="$CATALINA_OPTS -Xdebug
-Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=20002"

conf
~~~~

包含不同的配置文件

server.xml
^^^^^^^^^^

tomcat的主配置文件，包含service,connectors,engine,realm,valve,hosts等组件

::

 <Server port="8005" shutdown="SHUTDOWN">

Server是Tomcat中最顶层的组件，它可以包含多个Service组件，port指定一个端口，这个端口负责监听关闭tomcat的请求，shutdown指定向端口发送的命令字符串

::

 <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="off" />

默认支持Apr模式

::

 <Service name="Catalina">

Service组件相当于Connetor和Engine组件的包装器，它将一个或者多个Connector组件和一个Engine建立关联，name指定service的名字

::

 <Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" address="127.0.0.1" />

Connector [#]_ 是Tomcat中监听TCP网络连接的组件，一个Connector会监听一个独立的端口来处理来自客户端的连接，port指定服务器端要创建的端口号，并在这个断口监听来自客户端的请求，protocol
启动协议，maxThreads:设定在监听端口的线程的最大数目,这个值也决定了服务器可以同时响应客户请求的最大数目.默认值为200，acceptCount指定当所有可以使用的处理请求的线程数都被使用时，可以放到等待队列中的请求数，超过这个数的请求将不予处理.默认值为100，minProcessors服务器启动时创建的处理请求的线程数，maxProcessors最大可以创建的处理请求的线程数，minSpareThreads最小备用线程，maxSpareThreads最大备用线程，connectionTimeout指定超时的时间数(以毫秒为单位)，redirectPort指定服务器正在处理http请求时收到了一个SSL传输请求后重定向的端口号，enableLookups如果为true，则可以通过调用request.getRemoteHost()进行DNS查询来得到远程客户端的实际主机名；若为false则不进行DNS查询，而是返回其ip地址，address如果服务器有两个以上IP地址,该属性可以设定端口监听的IP地址,默认情况下,端口会监听服务器上所有IP地址，debug日志等级
，disableUploadTimeout禁用上传超时，主要用于大数据上传时

::

 <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />

AJP表示Apache Jserv Protocol,此连接器将处理Tomcat和Aapache
http服务器之间的交互，这个连机器是用来处理我们将Tomcat和Apache
http服务器结合使用的情况。假如在同样的一台物理Server上面部署了一台Apache
http服务器和多台Tomcat服务器，通过Apache服务器来处理静态资源以及负载均衡的时候，针对不同的Tomcat实例需要AJP监听不同的端口

::

 <Engine name="Catalina" defaultHost="localhost">

一个Engine可以包含一个或者多个Host,也就是说我们一个Tomcat的实例可以配置多个虚拟主机，name定义Engine容器的名称,
对应$CATALINA_HOME/config/Catalina中的Catalina，defaultHost指定缺省的处理请求的主机名，它至少与其中的一个host元素的name属性值是一样的，debug日志等级

::

 <Realm className="org.apache.catalina.realm.LockOutRealm">
 <Realm className="org.apache.catalina.realm.UserDatabaseRealm" resourceName="UserDatabase"/>
 <Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">

name指定主机名，一个虚拟主机可以有多个Context。appBase为webapps，也就是\\webapps目录，
unpackingWARS属性指定在appBase指定的目录中的war包都自动的解压，autoDeploy属性指定是否对加入到appBase目录的war包进行自动的部署

::

 <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs" prefix="localhost_access_log." suffix=".txt" pattern="%h %l %u %t "%r" %s %b" />

Valve中文意思是阀门，Valve是Tomcat中责任链模式的实现，通过链接多个Valve对请求进行处理。其中Valve可以定义在任何的容器中，上面说的Engine,Host,Context都属于容器。className
指定Valve使用的类名。默认定义了一个名为org.apache.catalina.valves.AccessLogValve的类,负责拦截每个请求，然后记录应用程序的访问信息志。directory
指定log文件存放的位置，pattern
有两个值，common方式记录远程主机名或ip地址，用户名，日期，第一行请求的字符串，HTTP响应代码，发送的字节数。combined方式比common方式记录的值更多。建议使用pattern="%{X-Real-IP}i
%l %u %t "%r" %s %b %{User-Agent}i" ，记录真实IP。

::

 <Context docBase="ROOT" path="" debug ="0" allowLinking="true" />

在Tomcat中，每一个运行的webapp其实最终都是以Context的形成存在，每个Context都有一个根路径和请求URL路径，
在Tomcat中我们通常采用如下的两种方式创建一个Context：在\\webapps目录中创建一个目录，这个时候将自动创建一个context，默认context的访问url为http://host:port/dirname
；在server.xml文件的元素中增加context元素。这样的话，我们就可以通过http://host:port/mypath访问上面配置的context了。

web.xml
^^^^^^^

遵循Servlet规范标准的配置文件，用于配置servlet，并为所有的Web应用程序提供包括MIME映射等默认配置信息

::

 <error-page>
   <error-code>404</error-code>
   <location>/404.html</location>
 </error-page>
 <error-page>
   <error-code>500</error-code>
   <location>/500.html</location>
 </error-page>

catalina.properties
^^^^^^^^^^^^^^^^^^^

Tomcat内部package的定义及访问相关的控制，也包括对通过类装载器装载的内容的控制；Tomcat在启动时会事先读取此文件的相关设置

`Class Loader
HOW-TO <http://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html>`__

`理解Tomcat的Classpath-常见问题以及如何解决 <http://www.linuxidc.com/Linux/2011-08/41684.htm>`__

lib
~~~

存放tomcat所需的jar文件

logs
~~~~

存放日志文件

temp
~~~~

临时文件

webapp
~~~~~~

存放应用程序示例，部署的应用程序也要放到此目录

work
~~~~

存放jsp编译后产生的class文件

connector模式
-------------

tomcat的connector模式有3种

-  HTTP connector

基于HTTP协议，负责建立HTTP连接。它又分为BIO http connector与NIO http
connector两种。

#. BIO(blocking I/O) HTTP connector
   默认的模式,阻塞式I/O操作,性能非常低下,没有经过任何优化处理和支持.
#. NIO(new I/O) HTTP connector
   是一个基于缓冲区、并能提供非阻塞I/O操作的Java
   API，因此nio也被看成是non-blocking
   I/O的缩写,拥有比传统I/O操作(bio)更好的并发运行性能.修改 connector
   节点中的 protocol 为 org.apache.coyote.http11.Http11NioProtocol 即可

-  AJP connector

基于AJP协议，AJP是专门设计用来为tomcat与http服务器之间通信专门定制的协议，能提供较高的通信速度和效率。如与Apache服务器集成时，采用这个协议。

-  APR HTTP connector [#]_

从系统级别来解决异步的IO问题,大幅度的提高性能，默认支持，但需要安装APR
library \| JNI wrappers for APR used by Tomcat (libtcnative) [#]_ \|
OpenSSL libraries

::

 apt-get install libapr1-dev libssl-dev
 tar zxvf tomcat/bin/tomcat-native.tar.gz
 cd tomcat/bin/tomcat-native-1.1.*-src
 ./configure --with-apr=/usr/bin/apr-1-config \
            --with-java-home=/usr/bin/java \
            --with-ssl=yes \
            --prefix=/usr/
 make && make install

libtcnative 将安装至 /usr/lib

Tomcat connectors 会自动开启对 APR 模式

.. rubric:: 参考文献

.. [#] `Tomcat总体结构 <http://imtiger.net/blog/2013/10/16/tomcat-architecture/>`_
.. [#] `The HTTP Connector <http://tomcat.apache.org/tomcat-7.0-doc/config/http.html>`_
.. [#] `Apache Portable Runtime (APR) based Native library for omcat <http://tomcat.apache.org/tomcat-7.0-doc/apr.html>`_
.. [#] `Tomcat Native <http://tomcat.apache.org/native-doc/>`_
