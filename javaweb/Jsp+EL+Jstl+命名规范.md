##JSP
- ####JSP脚本元素
	- 脚本:`<% 程序代码  %>`
	- 表达式:`<%= 表达式  %>`
	- 声明:`<%! 代码内容  %>`
	- 注释:`<%-- 注释内容 --%>`
- ####JSP指令
	- page指令,指明与JSP容器沟通的方式:`<%@page %>`
	- include指令,JSP页面被转换为Servlet之前将指定的文件包含进来:`<%@include %>`
	- taglib指令,导入标签库:`<%@ taglib>`
- ####JSP标签
	- `<jsp:include page='' flush='false'>`:包含一个静态或者动态的文件.(flush指定是否将当前页面的输出内容先刷新到客户端)
	- `<jsp:forward page=''>`:转发
	- `<jsp:param value='' name=''>`:给include,forward,plugin标签传递参数(通过request的param获取)(解决乱码,在传递的主页面中设置request的编码)
	- `<jsp:useBean id='' scope='' class=''>`:创建一个Bean实例,并指定名字(id)和作用范围(scope)
	- `<jsp:getProperty name='' property=''>`:获取一个Bean的属性值,并将其转换为字符串输出
	- `<jsp:setProperty name='' property='' value=''>`:设置一个Bean的属性值
- ####JSP内置对象(9个)
	- `out`:页面输出(JspWriter,相当于一个带缓存的PrintWriter)
	- `request`:用户请求
	- `response`:客户端响应
	- `config`:Servlet配置(ServletConfig)
	- `session`:Session对象
	- `application`:servletContext对象
	- `pageContext`:表示当前Jsp页面的运行环境
		- 可以通过pageContext获取其他8大内置对象
		- 操作域对象(page域,request域,session域,application域)
	- `page`:将该jsp转换为Servlet的实例
	- `exception`:表示Jsp所发生的异常,在错误页面中才起作用

##EL
- ####EL的内置对象
	- 作用域
		- `pageScope`
		- `requestScope`
		- `sessionScope`
		- `applicationScope`
	- 请求参数
		- `param`:获得一个参数(在请求`...?name=a&name=b`中,`${param.name} = a`)
		- `paramValues`:获得一组参数(`${paramValues.name[0]} = a`)
	- 请求头
		- `header`:获得一个请求头
		- `headerValues`:获得一组请求头
	- JSP上下文对象
		- `pageContext`
	- 全局初始化参数
		- `initParam`
	- Cookie
		- `cookie`
- ####EL用于获取数据
	- 获取bean的属性
		- `${属性名}`:会调用`pageContext.findAttribute`,分别从page,request,session,application四个域中查找相应的对象,找不到返回`""`;
		- `${属性名.属性名.属性名}`
	- 获取list集合中指定位置的数据
		- `${list[1]}`
	- 获取map集合中的数据
		- `${map.attr}`
		- `${map["attr"]}`
- ####EL执行运算
	- 算数运算符
		- `+`
		- `-`
		- `*`
		- `/`
		- `%`
	- 关系运算符
		- `== 或 eq`
		- `!= 或 ne`
		- `< 或 lt`
		- `> 或 gt`
		- `<= 或 le`
		- `>= 或 ge`
	- 逻辑运算符
		- `&& 或 and`
		- `|| 或 or`
		- `! 或 not`
	- `empty`运算符:检查对象是否为null
	- 二元表达式:`${user!=null ? user.name : ""}`
	- `[]`和`.`号运算符
- ####EL调用Java方法...
- ####拓展
	- 判断是否为空字符串(`empty 值`)
	- 判断是否为空(`值 == null`)

##JSTL
- ####表达式控制标签
	- out标签
		- `<c:out value='' [excapeXml='true|false'] [default='默认值'] />`
		- 类似`out.println('字符串')`
		- excapeXml代表是否将`>,<,&`等特殊字符进行HTML编码转换后再输出,默认true
			- true:
				- `value='<>'`时输出为`&lt;&gt;`
				- `value='&gt'`时输出为`&gt`
			- false:
				- `value='<>'`时输出为`<>`
				- `value='&gt'`时输出为`>`
		- default代表默认值,value为null的情况下
	- set标签(scope默认page)
		- `<c:set value='' var='变量名' [scope='作用域'] />`设置作用域属性
		- `<c:set target='对象' property='属性名' value='' />`设置对象的属性
	- remove标签
		- `<c:remove var='变量名' [scope='作用域'] />`
	- catch标签
		- `<c:catch [var='变量名']>嵌套内容</c:catch>`
		- 捕获嵌套在标签体中的内容抛出的异常
		- var标识捕获的异常对象,保存在page域中.
- ####流程控制标签
	- if标签
		- `<c:if test='条件' var='变量名' [scope='作用域']> 标签体内容</c:if>`
		- var用于存储判断的结果
	- choose,when,otherwise标签
		```
        <c:choose>
            <c:when test="条件1">
              //业务逻辑1
            </c:when>
            <c:when test="条件2">
              //业务逻辑2
            </c:when>
            <c:when test="条件n">
              //业务逻辑n
            </c:when>
            <c:otherwise>
              //业务逻辑
            </c:otherwise>
        </c:choose>
        ```
- ####循环标签
	- forEach标签
		```
        <c:forEach>
			var='存储从集合中取出来的元素'
            items='遍历的集合'
            begin='起始位置'
            end='终止位置'
            step='循环的步长'
            varStatus='存放当前遍历元素的状态'
        </c:forEach>
        ```
        - 用于遍历list或map集合
        - varStatus:
        	- index当前元素所在位置
        	- count已迭代的次数
        	- first是否为第一个元素
        	- last是否为最后一个元素
	- forTokens标签
		```
        	<c:forTokens>
            	items="遍历的字符串"
                delims="分割符"
                var="存储遍历到的成员"
                begin="开始位置"
                end="终止位置"
                step="步长"
                varStatus="状态信息"
            </c:forTokens>
        ```
        - 用于遍历字符串,并根据指定的字符将字符串截取
- ####URL操作标签
	- import标签
		```
        	<c:import
            	url="url"
                [context=""]
                [value=""]
                [scope=""]
                [charEncoding=""]
            />
        ```
        - 用于包含其他静态或动态文本包含到本JSP页面中,与`<jsp:include>`的区别为:
        	- `<jsp:include>`只能包含同一个web应用中的文件.
        	- `<c:import>`可以包含其他web应用中的文件,甚至网络上的资源
        - `url='/bb.txt'`表示当前项目根目录的bb.txt的文件
        - `context='/root'`表示root项目下的资源
	- url标签
		```
        	<c:url value="地址" var="变量名" scope="作用域">
            	<c:param name="参数名" value="值" />
            </c:url>
        ```
        - 构造一个URL地址并存储
    - redirect标签
    	```
        	<c:redirect url="" [context=""]>
				<c:param name="参数名" value="值" />
            </c:redirect>
        ```
        - 实现请求重定向
    - param标签
    	- 可以嵌入import,url,redirect标签中提供附加参数.

##命名规范
- Jsp页面命名
	- 以小写字母开头,如果多个单词,后面的单词以大写字母开头,需要体现页面的意义
- 类的注释
	- 基本功能
	- 作者
	- 版本
	- 日期
	- 公司名称
	- 版权声明
- 方法注释:功能,参数,返回值
- JavaEE项目文件夹命名
	- `images`:存放web程序所需公共图片
	- `css`:存放web程序所需公共样式表
	- `js`:存放web程序所需的公共js文件
	- `commons`:存放web程序所需的公共文件
	- `功能模块文件夹`:存放某个工鞥呢模块相关的资源
		- `images`
		- `css`
		- `js`
	- jsp,html页面
	- WEB-INF
		- classes
		- lib
		- tld文件

- package命名(一)
	- indi:(copyright属于发起者)
		- `indi.发起者名.项目名.模块名.xxx`
		- 个人项目,个人发起,非独立完成,可公开或私有
	- pers:(copyright属于个人)
		- `pers.个人名.项目名.模块名.xxx`
		- 个人项目,个人发起,独自完成,可分享
	- priv:(copyright属于个人)
		- `priv.个人名.项目名.模块名.xxx`
		- 私有项目,个人发起,独自完成
	- team:(copyright属于团队)
		- `team.团队名.项目名.模块名.xxx`
		- 团队项目,由团队发起,团队开发
	- com:(copyright属于公司)
		- `com.公司名.项目名.模块名.xxx`
		- 公司项目,公司发起

- package命名(二)
	- servlet类:`web.servlet`
	- 自定义标签类:`web.tags`
	- 过滤器类:`web.filter`
	- Action类:`web.struts.action`
	- JavaBean实现接口:`web.service`
	- dao接口:`dao`
	- dao实现类:`dao.impl`
	- POJO类与hbm文件:`dao.hbm`
	- 工具类:`util`