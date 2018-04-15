##ClassLoader
####定义
- Java中所有的类，必须装在到jvm中才能运行，这个装载的过程是由ClassLoader(类加载器)完成的，工作实质就是把类文件从硬盘读取到内存中。（通过ClassLoader的loadClass()方法加载class文件使用双亲委派模式）

####JVM平台提供三层ClassLoader
- Bootstrap ClassLoader
    - 采用native code实现，是JVM的一部分
    - 主要加载JVM自身工作需要的类，如java.lang.*等（`sun.boot.class.path`搜索）。
    - 不是一个普通的Java类，底层由C++编写。
    - JVM会先使用BootstrapClassLoader载入一个Launcher。
- ExtClassLoader
    - 加载位于$JAVA_HOME/jre/lib/ext目录下的扩展jar（`java.ext.dirs`搜索）
- AppClassLoader
    - 系统ClassLoader，父加载器为ExtClassLoader
    - 加载$CLASSPATH下的jar（`java.class.path`搜索）（加载用户自己创建的类）

####双亲委派模式
- 当一个类加载器加载类的时候，先委托父加载器加载，父加载器无法加载的时候，再自己加载。（`ClassLoader`的`loadClass`方法已经实现）
- 作用：避免用户自定义的类入侵系统的类
- ClassLoader的实现过程`loadClass`：
	- 1.`findLoadedClass`判断是否已经加载过了
	- 2.`parent.loadClass`调用父加载器加载
	- 3.`findBootstrapClassOrNull`若是没有父类加载器，则调用根加载器加载
	- 4.`findClass`若是都没有加载过，则调用自己的findClass加载
	- 5.根据resolve标记，判断是否调用`resolveClass`

####自定义的类加载器
- 作用：加载非classpath路径下的类
- 相关方法（ClassLoader）：
    - `loadClass`：加载类，无需重写，ClassLoader抽象类已经实现了双亲委派
    - `findClass`：查找需要加载的类，主要重写
    - `defineClass`：将一个字节数组转换为Class对象
- 实现方式
    - 实现ClassLoader抽象类
    - 继承URLClassLoader类


####类加载的方法：
- 隐式加载
- 显示加载
	- `ClassLoader.loadClass`
	- `Class.forName`


####ClassLoader加载类的过程
- 找到class文件，并把这个文件加载到内存中
- 字节码验证，Class类数据结构分析，内存分配和符号表的链接
- 类中静态属性初始化赋值以及静态代码块的执行


####热部署
- 定义：运行时更新java类文件
- 判断一个类是否同一类的两个条件
    - 类的完整类名是否一样(包括包名)
    - 加载这个类的类加载器是否是同一个
- 原理
    - 创建不同的ClassLoader实例对象，加载同名的类


####ContextClassLoader
- 每一个Thread都有一个相关联的Context ClassLoader。
- 没有主动设置的话，会继承父thread的ContextClassLoader
- 作用
    - 用于解决多线程环境中不同的对象是由不同的ClassLoader加载的，当一个对象由一个线程传到另一个线程的时候，想加载除开自己classpath以外的资源。（在框架代码中用的多）