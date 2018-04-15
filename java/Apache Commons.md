##Apache Commons

####BeanUtils
- 作用：把数据封装到对象的属性中（需要提供getter和setter方法）
- 常用API：
	- `BeanUtils`：工具类
		- `setProperty(Object obj, String attrName, Object value);`：给指定对象的属性存储值
		- `populate(Object obj, Map<String,? extends Object> map)`：将Map集合中的数据存储到对象中
		- `copyProperties(Object new, Object old)`：对象属性的拷贝
		- `Object cloneBean(Object bean)`：对象的克隆
	- `ConvertUtils`：转换器（属性类型和存储的数据类型不一致）（转换器需要实现Converter）
		- `register(Converter ,Class )`：注册指定类的数据转换器。

####Codec
- 作用：提供常用的编解码的方法。（MD5，Base64，URL等）
- Base64：
	- 作用：将非ASCII字符转换为ASCII字符。在网络传输中，那个ASCII为128-255之间的值是不可见的，由于不同的设备对字符的处理方式不同，那些不可见的字符就有可能被错误处理，所有将不可见的字符转换为可见的字符。
	- 原理：将8位二进制拆分为6位的片段，每6位片段分配一个字符，意味着8个字符表示信息的6位。若是不能均匀的分成6位的块，则在后面填充0，使长度成为24的倍数（6和8的最小公倍数）
	- API：`Base64的encodeBase64和decodeBase64`
- MD5:
	- 作用：消息摘要算法，用以提供消息的完整性保护。
	- 特点：
		- 压缩性：任意长度的数据，算出的MD5值长度都是固定的。
		- 容易计算：从原数据计算出MD5值很容易
		- 抗修改性：对原数据进行任何改动，所得到的MD5值都有很大的区别
		- 强抗碰撞：已知原数据和其MD5值，想找到一个具有相同MD5值的数据是非常困难的，意味着难以伪造数据
	- API：`DigestUtils.md2Hex(数据流，字符串，字节数组)`
- URL:
	- URL的字符：
		- 需要通过URL转码的字符，比如汉字等
		- 不需要通过URL转码的字符，比如12等
		- 需要处理的保留字，比如#等
	- API：
	```java
        String message = "xxx";
        URLCodec codec = ne URLCodec();
        String enMessage = codec.encode(message);
        String deMessage = codec.decode(enMessage);
    ```


####Collections
- 作用：对Java中集合类进行了一定的补充，定义了一些全新的集合，提供了一些常用的集合操作
- 常用API：(ListUtils,CollectionUtils)
	- 求交集
	- 求差
	- 求并集
- 注意：
	- ListUtils和CollectionUtils的方法存在一定的不同

####Betwixt
- 作用：XML与对象之间的相互转换

####Compress
- 作用：打包，压缩库

####Configuration
- 作用：帮助处理配置文件

####DBCP
- 作用：数据库连接池（依赖于commons-pool，Tomcat的数据源使用的就是DBCP）

####DbUtils
- 作用：JDBC工具类库，对JDBC的简单封装，可以将结果集转换为对象。

####Email
- 作用：对javamail的封装
- 样例
```java
    //1.初始化
    HtmlEmail mail = new HtmlEmail();
    mail.setHostName(HOSTNAME);					//设置邮件服务器域名
    //mail.setSmtpPort(aPortNumber); 				//设置邮件服务器smtp协议端口
    mail.setAuthentication(EMAIL, PASSWORD);	//邮件账户
    mail.setCharset(CHARSET);					//字符集

    mail.setSSLOnConnect(false);				//是否启用SSL
    //mail.setSslSmtpPort(sslSmtpPort);			//若启用，设置smtp协议的SSL端口
    mail.setFrom(EMAIL);						//发件人地址
    mail.addTo(RECEIVER_EMAIL);					//收件人地址

    //2.发送普通邮件
    mail.setSubject("Hello");					//邮件主题
    mail.setHtmlMsg("正文");						//邮件正文
    //3.发送带附件的邮件
    EmailAttachment attach = new EmailAttachment();		//附件对象
    attach.setPath(FILE);								//附件文件在系统中的路径
    attach.setDisposition(EmailAttachment.ATTACHMENT);
    mail.attach(attach);								//添加附件

    //4.发送内嵌图片的邮件(不能使用外部文件)
    String cid = mail.embed(new File(FILE));			//将图片嵌入邮件中，返回cid
    String img = "<img src='cid:" + cid + "' />";		//构造img标签，图片源为cid

    String htmlMsg = "这是图片i";
    htmlMsg = htmlMsg.replace("i", img);				//替换html邮件正文中的占位符
    System.out.println(htmlMsg);
    mail.setHtmlMsg(htmlMsg);
    mail.send();
```

- API：
	- 发送简单文本邮件`Email`
	- 发送带附件的邮件`MultiPartEmail`（可以添加网络上的文件到附件）
	- 发送HTML格式的邮件`HtmlEmail`
	- HTML内嵌图片相关：`embed()`和`<img src='cid:xxxx'/>`
	- 附件相关：`EmailAttachment对象`

####FileUpload
- 作用：java web文件上传功能

####HttpClient(HttpComponents)
- 作用：相比JDK自带的URLConnection，使发送Http请求变得简单。
	- `org.apache.commons.httpclient.HttpClient`不再被开发（HttpClient项目）
	- `org.apache.http.client.HttpClient`提供了更好的性能和更大的灵活性（HttpComponents项目）
- 开发步骤：
	```java
    	//1.创建HttpClient对象
		HttpClient client = HttpClients.createDefault();
		//2.创建Get请求
		HttpGet request = new HttpGet(targetUrl);
		//3.发送Get请求，获取响应
		HttpResponse res = client.execute(request);
		//4.获取响应实体
        HttpEntity entity = res.getEntity();
        //5.关闭连接，释放资源
        EntityUtils.consume(entity);
  ```
- [参考资料](http://itindex.net/detail/52566-httpclient)

####IO
- 作用：对java.io的扩展
- 功能：
	- 工具类
		- `FilenameUtils类`提供处理文件名（路径，文件名，扩展名）
		- `FileUtils类`提供文件操作（移动文件，读取文件，检查文件，遍历文件每一行）
		- `IOCase类`提供字符串操作和比较方法
		- `FileStore类`查看指定目录剩余空间
	- 文件监控器（获取文件的指定信息，监控文件的变化）
		- `FileEntry类`获取指定文件的信息
		- 文件监控
			- 1.创建File对象
			- 2.创建一个`FileAlterationObserver`对象，观察变化
			- 3.调用`add`方法，为observer对象添加一个FileAlterationListener对象
			- 4.创建一个`FileAlterationMonitor(毫秒值，观察者)`对象，将创建好的observer对象添加其中，并传入间隔时间
			- 5.调用start开启监视器
	- 过滤器
		- `NameFileFilter`名字过滤器
		- `WildcardFileFilter`通配符过滤器
		- `PrefixFileFilter`前缀过滤器
		- `SuffixFileFilter`后缀过滤器
		- `OrFileFilter`Or过滤器
		- `AndFileFilter`And过滤器
	- 比较器（排序，比较）
		- `NameFileComparator`名字比较器
		- `SizeFileComparator`大小比较器
		- `LastModifiedFileComparator`最新修改时间过滤器
	- 输入，输出（将输入流中的数据读取到输出流中）
		- `TeeInputStream`
		- `TeeOutputStream`可以分流

####Lang
- 作用：公共的工具集合，比如：字符，数组，字符串等。

####Logging
- 作用：
	- 能够自动选择使用哪些日志方式（log4j或JDK logging）
	- 提供了统一的接口,不与某个日志系统耦合
	- 想要使用log4j，将log4j的包放在classpath中即可。
	- commons-logging会自动发现并应用Log4j
- 使用：
```java
	private   static  Log log  =  LogFactory.getLog(YouClassName.class);
    /*
    	debug()		调试级别的日志信息
        info()		信息级别的日志信息
        warn()		警告级别的日志信息
        error()		错误级别的日志信息
        fatal()		致命错误级别的日志信息
    */
```
####Validator
- 作用：通用验证系统，提供了客户端和服务端的数据验证框架