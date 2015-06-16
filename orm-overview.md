# ORM 概览

## 什么是 JDBC？

JDBC 代表 **Java Database Connectivity** ，它是提供了一组 Java API 来访问关系数据库的 Java 程序。这些 Java APIs 可以使 Java 应用程序执行 SQL 语句，能够与任何符合 SQL 规范的数据库进行交互。

JDBC 提供了一个灵活的框架来编写操作数据库的独立的应用程序，该程序能够运行在不同的平台上且不需修改，能够与不同的 DBMS 进行交互。

## JDBC 的优点和缺点

|**JDBC 的优点**|**JDBC 的缺点**|
|:------------- |:-------------| 
|干净整洁的 SQL 处理|大项目中使用很复杂|
|大数据下有良好的性能|很大的编程成本|
|对于小应用非常好|没有封装|
|易学的简易语法|难以实现 MVC 的概念|
| |查询需要指定 DBMS|

## 为什么是对象关系映射（ORM）？

当我们工作在一个面向对象的系统中时，存在一个对象模型和关系数据库不匹配的问题。RDBMSs 用表格的形式存储数据，然而像 Java 或者 C\# 这样的面向对象的语言它表示一个对象关联图。考虑下面的带有构造方法和公有方法的 Java 类：

```
public class Employee {
   private int id;
   private String first_name; 
   private String last_name;   
   private int salary;  

   public Employee() {}
   public Employee(String fname, String lname, int salary) {
      this.first_name = fname;
      this.last_name = lname;
      this.salary = salary;
   }
   public int getId() {
      return id;
   }
   public String getFirstName() {
      return first_name;
   }
   public String getLastName() {
      return last_name;
   }
   public int getSalary() {
      return salary;
   }
}
```

现考虑以上的对象需要被存储和索引进下面的 RDBMS 表格中：

```
create table EMPLOYEE (
   id INT NOT NULL auto_increment,
   first_name VARCHAR(20) default NULL,
   last_name  VARCHAR(20) default NULL,
   salary     INT  default NULL,
   PRIMARY KEY (id)
);
```

第一个问题，如果我们开发了几页代码或应用程序后，需要修改数据库的设计怎么办？第二个问题，在关系型数据库中加载和存储对象时我们要面临以下五个不匹配的问题。

|**不匹配**|**描述**|
|:------------- |:-------------| 
|粒度|有时你将会有一个对象模型，该模型类的数量比数据库中关联的表的数量更多|
|继承|RDBMSs 不会定义任何在面向对象编程语言中本来就有的继承|
|身份|RDBMS 明确定义一个 'sameness' 的概念：主键。然而，Java 同时定义了对象判等（a==b）和 对象值判等（a.equals(b)）|
|关联|面向对象的编程语言使用对象引用来表示关联，而一个 RDBMS 使用外键来表示对象关联|
|导航|在 Java 中和在 RDBMS 中访问对象的方式完全不相同|

**O**bject-**R**elational **M**apping (ORM) 是解决以上所有不匹配问题的方案。

## 什么是 ORM？

ORM 表示 Object-Relational Mapping (ORM)，是一个方便在关系数据库和类似于 Java， C\# 等面向对象的编程语言中转换数据的技术。一个 ORM 系统相比于普通的 JDBC 有以下的优点。

|**序号**|**优点**|
|:------------- |:-------------| 
|1|使用业务代码访问对象而不是数据库中的表|
|2|从面向对象逻辑中隐藏 SQL 查询的细节|
|3|基于 JDBC 的 'under the hood'|
|4|没有必要去处理数据库实现|
|5|实体是基于业务的概念而不是数据库的结构|
|6|事务管理和键的自动生成|
|7|应用程序的快速开发|

一个 ORM 解决方案由以下四个实体组成：

|**序号**|**优点**|
|:------------- |:-------------| 
|1|一个 API 来在持久类的对象上实现基本的 CRUD 操作|
|2|一个语言或 API 来指定引用类和属性的查询|
|3|一个可配置的服务用来指定映射元数据|
|4|一个技术和事务对象交互来执行 dirty checking, lazy association fetching 和其它优化的功能|

## Java ORM 框架

在 Java 中有几个持久化的框架和 ORM 选项。一个持久化的框架是 ORM 存储和索引对象到关系型数据库的服务。

- Enterprise JavaBeans Entity Beans
- Java Data Objects
- Castor
- TopLink
- Spring DAO
- Hibernate
- And many more
