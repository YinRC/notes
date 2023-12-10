# 1. java 基础知识补充

+ 范型

# 2. Spring 框架

Spring

+ 控制反转 IOC【把流程的控制从应用程序转移到框架之中】 ---> 依赖注入 DI【set方法、构造方法】
+ 面向切面 AOP【代理模式 = 静态代理 + 动态代理】
+ Bean 的作用域 单例模式 和 原型模式
+ 自动装配：byName byType
+ 拦截器 -> 使用动态代理实现



SpringMVC

+ MVC：模型（dao，service）视图（jsp）控制器（servlet）
+ SpringMVC 三大件：**处理器映射器**，**处理器适配器**，**视图解析器**（jsp的完整路径）
+ RestFul
+ 请求转发与重定向
+ 过滤器 处理请求和响应 [可以处理权限问题，注销之后就不能进入主页了]
  + servletRequest.setCharacterEncoding("UTF-8");
  + servletResponse.setCharacterEncoding("UTF-8");
+ 保存会话状态：cookie[客户端技术：登陆状态] session[服务器技术：购物车]

# 3. 数据库相关

+ DDL -- 数据库定义语言
  + create alter drop table
+ DML -- 数据库操作语言
  + insert update delete
+ DQL -- 数据库查询语言*
  + select
+ DCL -- 数据库控制语言
  + 授予撤销访问权限，创建用户



+ 数据类型
  + int 4B
  + varchar 2～4B
  + datetime YYYY-MM-DD HH:mm:ss



+ 重要概念
  + 外键
  + 事务ACID原则：原子性，一致性，隔离性，持久性
  + 脏读和幻读
    + 脏读：读取了未提交的数据（隔离性违反）
    + 幻读：同一事务的多次查询结果不同（隔离性违反）
  + 事务：try sql catch conn.rollback() finally release

数据库引擎

+ InnoDB：重启数据库自增就会清零（存在内存当中，断电即失）
+ MyISAM：重启数据库后继续从上一个自增量开始（存在文件中，不会丢失）
+ InnoDB是一个支持事务的存储引擎，而MyISAM不支持事务只支持表级锁



+ PG数据库
  + 开源的
  + MySQL列名是用反引号，但是PG使用双引号
  + 字符串应该使用单引号
  + 对ACID的支持更好
  + 支持更多的索引算法，可以对性能进行更好的调整
  + 对象关系数据库
  + 频繁更新数据：PostgreSQL 会创建一个新的系统进程，为每个连接到数据库的用户分配大量内存（大约 10MB）。它需要内存密集型资源才能针对多个用户进行扩展
+ 达梦数据库
  + 闭源的
  + 不支持 if case-when-then-else
  + current_timestamp 的返回值带有时区
  + 不支持有些日期函数
+ MySQL数据库
  + 开源的
  + 频繁读取数据：MySQL 为多个用户使用单一进程。因此，对于主要向用户读取和显示数据的应用程序，MySQL 数据库的表现优于 PostgreSQL



+ JDBC（Java Database Connectivity）
  + 是一个数据库驱动
  + 对各个厂商的数据库驱动进行统一规范



+ 池化技术
  + 最小连接数
  + 最大连接数
  + 过多等待，超时断开
  + 用这个技术管理数据库的连接问题



# 4. MyBatis 持久层框架

+ 配置文件：配置数据源/映射Mapper文件/起别名
+ dao：对持久层操作的接口
+ mapper：写sql逻辑
+ mybatis-plus：querywrapper 拼接sql语句

# 5. 代码安全&性能

+ sql注入问题：PreparedStatement #{} 而不是 ${}
+ 不要在循环中用➕拼接字符串
+ 不要再循环中执行数据库操作

# 6. 单元测试

JUnit：@Test test1()



# 7. git



# 8. 业务

