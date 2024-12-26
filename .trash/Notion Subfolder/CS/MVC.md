- 视频时间 *2.5

  

- 源文件看不到注解，点击idea提示的download source

# Java

- SDK和Java Version
    - **Java语言版本**：
        - Java语言版本指的是Java语言的不同版本，如Java SE 8、Java SE 11、Java SE 17等。每个Java语言版本都有特定的语法规则、核心库和语言特性。这些版本由Java社区过程（Java Community Process, JCP）定义，并发布详细的规范。
        - 每个Java语言版本都会引入新的语言特性、改进现有的特性和可能废弃一些过时的特性。例如，Java SE 8引入了Lambda表达式和Stream API，而Java SE 11引入了本地变量类型推断（var关键字）。
    - **Java SDK**：
        - Java SDK是一个软件开发工具包，包含了用于开发Java应用程序的工具和库。Java SDK的一个常见实现是JDK（Java Development Kit）。JDK不仅包括Java编译器（javac）、Java运行时环境（JRE），还包括诸如调试器（jdb）、文档生成工具（javadoc）等开发工具。
        - 不同版本的JDK对应于不同的Java语言版本。例如，JDK 8支持Java SE 8，JDK 11支持Java SE 11。每个JDK版本都实现了相应Java语言版本的规范，并提供了支持这些规范的工具和库。
- 使用控制面板卸载JDK，不要手动删除文件夹！
- UUID: 无论何时何地`UUID.`_`randomUUID`_`()` 生成的UUID一定是不同的
    - [JAVA UUID 生成_java生成uuid方法-CSDN博客](https://blog.csdn.net/2401_84046577/article/details/137758384)

  

# Springboot

- http://localhost:8080/hello
    - /hello为请求的资源
- 添加@RequestController 等等按tab自动补全，会自动添加import
- json对象在请求和响应中，最外层的对象没有名字，数组的元素没有名字（数组本身有名字）。其余的都有名字，包括所有变量，对象，和成员
- Maven中类路径(classpath) 就是 Initializer自动生成的 resources文件夹
    - `String file = this.getClass().getClassLoader().getResource("emp.xml").getFile();`
    - 在resources目录下的 emp.xml 文件
    - 编译完成后会转存一份在 project 目录下的 target 文件夹
- URL中可以直接访问 类路径/static 下的静态页面，如html
    
      
    
- Stream API - forEach
    
    ```Java
    List<Emp> empList = XmlParserUtils.parse(file, Emp.class);
    // 对数据进行转换处理
    empList.stream().forEach(emp-> {
        // 处理第一个字段
        String gender = emp.getGender();
        if("1".equals(gender)) {
            emp.setGender("男");
        } else if("2".equals(gender)) {
            emp.setGender("女");
        }
    });
    ```
    
    就跟 for(Emp emp : empList) 差不多
    
- 一个实例只能注入一个Bean
- @Resource 和 @Autowired 的 区别：[https://blog.csdn.net/qq_44421796/article/details/110877497](https://blog.csdn.net/qq_44421796/article/details/110877497)
- Spring Boot 在启动时（例如通过 `mvn spring-boot:run` 或者直接运行主类）时，Spring Boot 并不会自动触发单元测试的执行。通常单元测试是在构建工具（如 Maven 或 Gradle）执行构建时触发的。例如，在 Maven 中，使用 `mvn package`或 `mvn verify` 命令会运行测试。而 `mvn clean install` 在构建项目的过程中也会执行所有测试，确保代码的正确性。

# Maven

- Project JDK, Maven Runner, Java Compiler 三者版本一致
    - 新建Project时选择的JDK将同时设置Project JDK和Java Compiler。Maven Runner “Use project SDK” 就一样了
    - Maven version 和 本地 repo 需要单独设置
- Maven的本地仓库相当于本地缓存，如果之前下载过这个版本的依赖，就会直接用本地仓库的。如果是第一次使用，就会先从 远程仓库（镜像）/中央仓库 去下载依赖。
- 依赖查找顺序：本地仓库 → 私服 → 中央仓库
- Maven 依赖信息：[https://mvnrepository.com](https://mvnrepository.com)

```XML
 <properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>
```

这个是你自己项目的Java版本。

- 明明已经下载jar包，IDE import也没报错，但运行后显示找不到包
    
    ![[_attachments/Untitled 181.png|Untitled 181.png]]
    
    file→invalidate cache→invalidate and restart. 重启后reload Maven project即可解决。
    
    ![[_attachments/Untitled 1 41.png|Untitled 1 41.png]]
    
    [报错 java: 程序包org.springframework.boot不存在 的一个解决办法-CSDN博客报错 java: 程序包org.springframework.boot不存在 的一个解决办法-CSDN博客](https://blog.csdn.net/tg928600774/article/details/121605260)
    
- 国内下载慢，换源阿里云
    
    ```Java
    <mirror>  
      <id>alimaven</id>  
      <name>aliyun maven</name>  
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>          
    </mirror>
    ```
    
- 继承
    - 子工程会继承父工程的groupID
    - 父工程 <dependencies> 中的依赖**所有**子工程自动继承， <dependencyManagement> 中的依赖只是用来版本锁定，子工程**不会直接继承**这些依赖，但如果子工程使用了某些依赖就不需要再指定版本，直接沿用父工程指定的统一版本。
- 生命周期中package和install的区别
    - `package` 阶段负责将项目的编译产物（如 `.class` 文件）打包成一个可分发的格式，通常是 JAR 文件或 WAR 文件。（项目目录下的 target 文件夹）
    - `install` 阶段负责将打包好的 JAR 文件或 WAR 文件**安装**到本地 Maven 仓库中。
    - `clean` 会清除 target 目录下的临时包，但不会清除本地 Maven 仓库中的包
- 上传父工程到私服会带着上传所有聚合子工程。下载子工程会带着下载父工程

# MySQL

![[_attachments/MySQL%E5%AE%89%E8%A3%85%E6%96%87%E6%A1%A3 3.pdf|MySQL%E5%AE%89%E8%A3%85%E6%96%87%E6%A1%A3 3.pdf]]

- 用户: root，默认密码：1234，默认端口号: 3306
- 命令行登录MySQL: `mysql -uroot -p<password>`
    - 以`root` 用户登录mysql
- 可视化工具：[Day06-05. MySQL-DDL-图形化工具_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1m84y1w7Tb?p=84&spm_id_from=pageDriver&vd_source=be8a69263c5fc7d89f00355a93e8a346)
- 如何在idea中连接到mysql数据库
    - 右侧边Database→data source→MySQL
    - 端口号默认，User: root, Password:
- 展示出哪些数据库：点击数据库右边的数字
    
    ![[_attachments/Untitled 2 24.png|Untitled 2 24.png]]
    
- 如何执行单行SQL：选中SQL再点绿色箭头执行
- session和console，session就相当于一次登录进程，console记录SQL
    
    ![[_attachments/Untitled 3 16.png|Untitled 3 16.png]]
    
- 如何访问之前的查询控制台和SQL记录
    
    ![[_attachments/Untitled 4 12.png|Untitled 4 12.png]]
    
- 如何运行SQL script，右键运行的数据库→SQL Scripts
    
    点击上方最大的绿色按钮
    
    ![[_attachments/Untitled 5 11.png|Untitled 5 11.png]]
    
- Ctrl+Alt+L 格式化一条SQL语句
- 内连接（隐式）+额外条件
    
    ```SQL
    select d.name, d.price, c.name
    from dish d,
         category c
    where d.category_id = c.id
      and d.price < 10;
    ```
    
    where 中 and 连接
    
- 外连接（隐式）+ 额外条件
    
    ```SQL
    select d.name, d.price, c.name
    from dish d
             left join category c on d.category_id = c.id
    where d.price between 10 and 50
      and d.status = 1;
    ```
    
    on + where
    
- 分组查询group by，能查询的字段就只有用来分组的字段和聚合函数（确保每一组只有一个数据）
- 多对多内连接
    
    dish, setmeal为业务表，setmeal_dish为中间表
    
    ```SQL
    select s.name, s.price, d.name, d.price, sd.copies
    from setmeal s,
         setmeal_dish sd,
         dish d
    where s.id = sd.setmeal_id
      and sd.dish_id = d.id
      and s.name = '商务套餐A';
    ```
    
- 在MySQL中，当你提交（`COMMIT`）或回滚（`ROLLBACK`）事务之后，**事务就结束了**。接下来的任何操作都将在自动提交模式（autocommit mode）下执行，默认情况下，每条语句都作为一个独立的事务立即提交到数据库。

- SUM函数，如果查询的集为空或数据都为NULL，返回的结果为NULL。有任何非NULL的值，SUM都会返回求和。
    
    ```SQL
    SELECT IFNULL(SUM(award_amount), 0) AS amount
    from commission_accounting_detail
    where settlement_id=#{settlementId} and accounting_status=#{accountingStatus}
    ```
    

  

# Mybatis

- Springboot 配置数据库，在application.properties 中
    
    ```SQL
    spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
    
    spring.datasource.url=jdbc:mysql://localhost:3306/mybatis
    
    spring.datasource.username=root
    
    spring.datasource.password=1234
    ```
    
- int 对应 Integer, bigint 对应 long, tinyint 对应 Short, varchar 对应 String。date对应LocalDate，datetime对应LocalDateTime
- SQL语句语法报错，连接数据库的时候必须指定Database
- 切换Druid数据库连接池
    1. Maven中引入依赖。注意要用Spring3 starter。[Maven Repository: com.alibaba » druid-spring-boot-3-starter (mvnrepository.com)](https://mvnrepository.com/artifact/com.alibaba/druid-spring-boot-3-starter)
    2. 之后就自动启用了
- 实体类成员变量名必须与字段名如果不一致，查询结果字段是不能自动封装到实体类的。如果字段名和成员变量名内容一致只是下划线命名转到驼峰命名，可以通过如下方式自动封装。
- 驼峰命名自动映射
    
    ![[_attachments/Untitled 6 11.png|Untitled 6 11.png]]
    
    ```Java
    mybatis.configuration.map-underscore-to-camel-case=true
    ```
    
- 插入实体类
    
    ```Java
    @Options(useGeneratedKeys = true, keyProperty = "Id123")
    @Insert("insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time)" +
            "values (#{username}, #{name}, #{gender}, #{image}, #{job}, #{entrydate}, #{deptId}, #{createTime}, #{updateTime});")
    public void insert(Emp emp);
    ```
    
    keyProperties = “” 是成员变量名，emp() 括号内的是字段名，#{} 大括号内的是成员变量名
    
- 动态SQL
    - 非必须参数
        - <if>
        - select使用<where>标签，可以不给任何条件，where会被自动清除
        - update使用<set>标签，不能不给任何更新值，SQL语法不允许不update任何东西。
    - 数组参数
        - <foreach>
- 无论是在Mapper注释中还是xml文件中，SQL语句最后都不要加;

# 案例

- 在Service层完成分页
- PageHelper插件: 使用插件之前需要 count(*) + limit 两条SQL。pageHelper插件仅需要 select 语句，在你调用 list SQL 时，插件实际执行了两句SQL，第一句在select中加上count(*)，第二句在 select最后加上 limit a, b
- 阿里云OSS endpoint地址在bucket列表中的概览，访问端口，外网访问
- 配置文件 [application.properties](http://application.properties) 中字符串不需要加引号，每行最后不需要加分号 ;
- @ConfigurationsProperties 对象中的成员是驼峰命名，对应的 yaml 文件中的 key 名可以是驼峰命名也可以是横杠 - 命名