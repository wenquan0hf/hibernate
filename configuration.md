# 配置

Hibernate 需要事先知道在哪里找到映射信息，这些映射信息定义了 Java 类怎样关联到数据库表。Hibernate 也需要一套相关数据库和其它相关参数的配置设置。所有这些信息通常是作为一个标准的 Java 属性文件提供的，名叫 **hibernate.properties**。又或者是作为 XML 文件提供的，名叫 **hibernate.cfg.xml**。 
  
我会对 **hibernate.cfg.xml** 这个 XML 格式文件进行考虑，来在我的例子里指定需要的 Hibernate 应用属性。这个格式文件中大多数的属性都有它们应有的价值所以并不需要特意去指定它们，除非真的是有需要。这个文件保存在应用程序的类路径的根目录里。

## Hibernate 属性

下面是一个重要的属性列表，你可能需要表中的属性来在单独的情况下配置数据库。

|**S.N.**|**属性和描述**|
|:------------- |:-------------| 
|1| **hibernate.dialect** <br>这个属性使 Hibernate 应用为被选择的数据库生成适当的 SQL。|                                                     
|2|	**hibernate.connection.driver_class**<br> JDBC 驱动程序类。|
|3|	**hibernate.connection.url** <br>数据库实例的 JDBC URL。|
|4|	**hibernate.connection.username**<br> 数据库用户名。|
|5|	**hibernate.connection.password** <br>数据库密码。|
|6|	**hibernate.connection.pool_size** <br>限制在 Hibernate 应用数据库连接池中连接的数量。|
|7|	**hibernate.connection.autocommit** <br>允许在 JDBC 连接中使用自动提交模式。|

如果您正在使用 JNDI 和数据库应用程序服务器然后您必须配置以下属性:

|**S.N.**|**属性和描述**|
|:------------- |:-------------| 
|1|	**hibernate.connection.datasource** <br>在应用程序服务器环境中您正在使用的应用程序 JNDI 名。|
|2|	**hibernate.jndi.class** <br>JNDI 的 InitialContext 类。|
|3|	**hibernate.jndi.&lt;JNDIpropertyname&gt;** <br>在 JNDI的 InitialContext 类中通过任何你想要的 Java 命名和目录接口属性。|
|4|	**hibernate.jndi.url** <br>为 JNDI 提供 URL。|
|5|	**hibernate.connection.username** <br>数据库用户名。|
|6|	**hibernate.connection.password** <br>数据库密码。|

## Hibernate 和 MySQL 数据库

MySQL 数据库是目前可用的开源数据库系统中最受欢迎的数据库之一。我们要创建 **hibernate.cfg.xml** 配置文件并将其放置在应用程序的 CLASSPATH 的根目录里。你要确保在你的 MySQL 数据库中 **testdb** 数据库是可用的，而且你要有一个用户 **test** 可用来访问数据库。

XML 配置文件一定要遵守 Hibernate 3 Configuration DTD，在 http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd. 这个网址中是可以找到的。  

``` 
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-configuration SYSTEM 
"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
   <session-factory>
   <property name="hibernate.dialect">
      org.hibernate.dialect.MySQLDialect
   </property>
   <property name="hibernate.connection.driver_class">
      com.mysql.jdbc.Driver
   </property>

   <!-- Assume test is the database name -->
   <property name="hibernate.connection.url">
      jdbc:mysql://localhost/test
   </property>
   <property name="hibernate.connection.username">
      root
   </property>
   <property name="hibernate.connection.password">
      root123
   </property>

   <!-- List of XML mapping files -->
   <mapping resource="Employee.hbm.xml"/>

</session-factory>
</hibernate-configuration> 
```

上面的配置文件包含与 hibernate-mapping 文件相关的 **&lt;mapping&gt;** 标签，我们将在下章看看 hibernate mapping 文件到底是什么并且要知道为什么用它，怎样用它。以下是各种重要数据库同源语属性类型的列表:

|**Database**|**Dialect Property**|
|:------------- |:-------------|  
|DB2|	org.hibernate.dialect.DB2Dialect  |
|HSQLDB|	org.hibernate.dialect.HSQLDialect  |
|HypersonicSQL|	org.hibernate.dialect.HSQLDialect  |
|Informix|	org.hibernate.dialect.InformixDialect  |
|Ingres|	org.hibernate.dialect.IngresDialect  |
|Interbase|	org.hibernate.dialect.InterbaseDialect  |
|Microsoft SQL Server 2000|	org.hibernate.dialect.SQLServerDialect  |
|Microsoft SQL Server 2005|	org.hibernate.dialect.SQLServer2005Dialect  |
|Microsoft SQL Server 2008|	org.hibernate.dialect.SQLServer2008Dialect  |
|MySQL|	org.hibernate.dialect.MySQLDialect  |
|Oracle (any version)|	org.hibernate.dialect.OracleDialect  |
|Oracle 11g|	org.hibernate.dialect.Oracle10gDialect  |
|Oracle 10g|	org.hibernate.dialect.Oracle10gDialect  |
|Oracle 9i|	org.hibernate.dialect.Oracle9iDialect  |
|PostgreSQL|	org.hibernate.dialect.PostgreSQLDialect  |
|Progress|	org.hibernate.dialect.ProgressDialect  |
|SAP DB|	org.hibernate.dialect.SAPDBDialect  |
|Sybase|	org.hibernate.dialect.SybaseDialect  |
|Sybase Anywhere|	org.hibernate.dialect.SybaseAnywhereDialec|