##JDK原生API能实现的功能
####获取Web服务器的资源，提交表单数据(Net)
- 相关类，接口
	- `java.net.URLConnection`：抽象类，代表应用程序和URL之间的通讯连接
	- `java.net.URL`：url的类，通过该类可以获取URLConnection对象
- 流程
	- 1.创建URL对象：`new URL(xxx)`
	- 2.调用URL对象的`openConnection`获取URLConnection连接对象：`url.openConnection()`
	- 3.设置是否用该连接进行输出到服务端(URLConnection的子类HttpURLConnection)（也就是表单提交）：`connection.setDoOutput(true)`
	- 4.获取请求体流对象： `connection.getOutputStream`
	- 5.对OutputStream写入数据
	- 6.连接web服务器：`connection.con()`
	- 7.获取相应头的map： `connection.getHeaderFields()`
	- 8.获取响应体的流对象： `connection.getInputStream()`
- 相关功能：
	- 上传
	- 下载