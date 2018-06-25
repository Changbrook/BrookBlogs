#### HTTP1.0与1.1区别

在HTTP1.0协议中，客户端与web服务器建立连接后，只能获得一个web资源。

在HTTP1.1协议，允许客户端与web服务器建立连接后，在一个连接上获取多个web资源。

#### serverlet相关

&lt;servlet&gt;元素用于注册Servlet，它包含有两个主要的子元素：&lt;servlet-name&gt;和&lt;servlet-class&gt;，分别用于设置Servlet的注册名称和Servlet的完整类名

由于客户端是通过URL地址访问web服务器中的资源，所以Servlet程序若想被外界访问，必须把servlet程序映射到一个URL地址上，这个工作在web.xml文件中使用&lt;servlet&gt;元素和&lt;servlet-mapping&gt;元素完成

