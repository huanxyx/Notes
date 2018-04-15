###JDBC
####驱动程序:Driver接口
- `Class.forName('com.mysql.jdbc.Driver')`:注册驱动

####驱动程序管理:DriverManager类
- `registerDriver(Driver)`注册驱动(无需使用)
- `static Connection getConnection(String url, String user, String password)`:获取连接对象

####数据库连接:Connection接口
- `Statement createStatement()`:创建Statement对象
- `PreparedStatement prepareStatement(String sql)`:创建PreparedStatement对象
- `CallableStatement prepareCall(String sql)`:创建CallableStatement对象
- `setAutoCommit(boolean)`:设置是否自动提交,打开事务
- `commit()`:提交事务
- `rollback()`:回滚事务

####数据库操作对象
- 三种Statement接口:
    - Statement接口
    - PreparedStatement接口
        - `setXXX(位置1-...,值)`
        - `setObject(位置,值)`:底层使用了instanceof判断
    - CallableStatement接口
- 常用方法:
    - `boolean execute(String sql)`:运行,返回是否有结果集
    - `ResultSet executeQuery(String sql)`:执行查询语句
    - `int executeUpdate(String sql)`:(执行DDL-create等,返回0)(执行DML,返回受影响的行数)
    - `addBatch`:把多条sql语句放到一个批处理中
    - `executeBatch()`:发送一批sql语句执行
    - `close()`:关闭

####结果集:ResultSet接口
- 移动
    - `fisrt()`定位到数据库记录的第一行
    - `last()`定位到数据库记录的最后一行
    - `afterLast()`将数据库游标移动到记录集最后一行的后面
    - `beforeFirst()`将数据库游标移动到记录集第一行的前面
    - `boolean next()`向后移动一个位置(第一次使用,为移动到第一行)(有记录则为true,没有记录则为false)
    - `boolean previous()`向前移动一个位置
    - `relative(int rows)`相对定位到指定行
    - `absolute(int rows)`绝对定位到指定行
- 判断位置
    - `isFirst()`判断当前行是否为记录的第一行
    - `isLast()`判断当前行是否为记录的最后一行
    - `isAfterLast()`判断是否为最后一行的后面
    - `isBeforeFirst()`判断是否为第一行的前面
- 获取某列的值
    - `getObject(列数1...)`
    - `getObject(列名)`
    - `getXXX(列数1...)`
    - `getXXX(列名)`
- 扩展
    - 获取行数的方法:last() -> getRow()
    - null结果值:在java中对象为null,基本类型为0,boolean为false
    - 用户不必关闭ResultSet,当产生它的Statement关闭,重新执行时,ResultSet将被Statement自动关闭.

####扩展
- 为了确保资源释放代码能运行,释放资源代码一定要放到finally语句中
- JDBC可以操作存储过程
- 数据库URL格式:`协议jdbc:子协议(mysql)://主机号:端口/数据库?参数名=参数值`
- [其他资料链接](https://www.cnblogs.com/erbing/p/5805727.html)