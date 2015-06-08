#Hibernate-图文件

一个对象/关系型图一般定义在 XML 文件中。图文件指示 Hibernate 如何将已经定义的类或类组与数据库中的表对应起来。  
尽管有些 Hibernate 用户选择手写 XML 文件，但是有很多工具可以用来给先进的 Hibernate 用户生成图文件。这样的工具包括 **XDoclet**, **Middlegen** 和 **AndroMDA**。  
让我们来考虑我们之前定义的 POJO 类，它的对象将延续到下一部分定义的表中。  

```
public class Employee {
    private int id;
    private String firstName; 
    private String lastName;   
    private int salary;  

    public Employee() {}
    public Employee(String fname, String lname, int salary) {
        this.firstName = fname;
        this.lastName = lname;
        this.salary = salary;
    }
    public int getId() {
        return id;
    }
    public void setId( int id ) {
        this.id = id;
    }
    public String getFirstName() {
        return firstName;
    }
    public void setFirstName( String first_name ) {
        this.firstName = first_name;
    }
    public String getLastName() {
        return lastName;
    }
    public void setLastName( String last_name ) {
        this.lastName = last_name;
    }
    public int getSalary() {
        return salary;
    }
    public void setSalary( int salary ) {
        this.salary = salary;
    }
}
```

对于每一个你想要提供持久性的对象都需要一个表与之保持一致。考虑上述对象需要存储和检索在下列 RDBMS 表中：  

```
create table EMPLOYEE (
    id INT NOT NULL auto_increment,
    first_name VARCHAR(20) default NULL,
    last_name  VARCHAR(20) default NULL,
    salary     INT  default NULL,
    PRIMARY KEY (id)
);
```

基于这两个实体之上，我们可以定义下列图文件来指示 Hibernate 如何将已定义的类或类组与数据库表匹配。  

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC 
 "-//Hibernate/Hibernate Mapping DTD//EN"
 "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd"> 

<hibernate-mapping>
   <class name="Employee" table="EMPLOYEE">
      <meta attribute="class-description">
         This class contains the employee detail. 
      </meta>
      <id name="id" type="int" column="id">
         <generator class="native"/>
      </id>
      <property name="firstName" column="first_name" type="string"/>
      <property name="lastName" column="last_name" type="string"/>
      <property name="salary" column="salary" type="int"/>
   </class>
</hibernate-mapping>
```

你需要以格式 <classname>.hbm.xml 保存图文件。我们保存图文件在 Employee.hbm.xml 中。让我们来详细的看一下图文件中使用的一些标签:  

- 图文件是一个以 **<hibernate-mapping>** 为根元素的 XML 文件，里面包含所有 <class> 标签。
- **<Class>** 标签是用来定义从一个JAVA类到数据库表的特定图。 JAVA 的类名使用 **name** 属性来表示，数据库表明用 **table** 属性来表示。
- **<meta>** 标签是一个可选元素，可以被用来修饰类。
- **<id>** 标枪将类中独一无二的 ID 属性与数据库表中的主键关联起来。 id 元素中的 **name** 属性引用类的性质， **column** 属性引用数据库表的列。 **type** 属性保存 Hibernate 图的类型，这个类型会将从 JAVA 转换成 SQL 数据类型。
-  在 id 元素中的 **<generator>** 标签用来自动生成主键值。设置 generator 标签中的 **class** 属性可以设置 **native** 使 Hibernate 可以使用 **identity**, **sequence** 或 **hilo** 算法根据底层数据库的情况来创建主键。
-  **<property>** 标签用来将 JAVA 类的属性与数据库表的列匹配。 标签中 **name** 属性引用的是类的性质，**column** 属性引用的是数据库表的列。**type** 属性保存 Hibernate 图的类型，这个类型会将从 JAVA 转换成 SQL 数据类型。

还有一些其他属性和元素可用在图文件中，我会在其他讨论 Hibernate 相关的主题中尽可能的涉及更多。
 
  
