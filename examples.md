# 例子
让我们看一个使用 Hibernate 来提高独立应用程序中的 Java 持久性的例子。我们将通过不同的步骤使用 Hibernate 技术创建 Java 应用程序。  

## 创建 POJO 类：

创建应用程序的第一步就是建立 Java 的 POJO 类或者其他类，这取决于即将要存放在数据库中的应用程序。我们可以考虑一下让我们的 **Employee** 类使用 **getXXX** 和 **setXXX** 方法从而使他们变成符合 JavaBeans 的类。  

POJO (Plain Old Java Object) 是 Java 的一个对象，这种对象不会扩展或者执行一些特殊的类并且它的接口都是分别在 EJB 框架的要求下的。所有正常的 Java 对象都是 POJO。  

当你设计一个存放在 Hibernate 中的类时，最重要的提供支持 JavaBeans 的代码和在 Employee 类中像 **id** 属性一样可以当做索引的属性。  

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

## 创建数据库表：

第二步就是在你的数据库中创建表格。每一个你所愿意提供长期留存的对象都会有一个对应的表。上述的对象需要在下列的 RDBMS 表中存储和被检索到：  

```
create table EMPLOYEE (
   id INT NOT NULL auto_increment,
   first_name VARCHAR(20) default NULL,
   last_name  VARCHAR(20) default NULL,
   salary     INT  default NULL,
   PRIMARY KEY (id)
);
```  

## 创建映射设置文件： 

这一步是创建一个映射文件从而指导 Hibernate 如何对数据库的表映射定义的类。  

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

你需要将映射文档以 <classname>.hbm.xml 的格式保存在一个文件中。我们将映射文档保存在 Employee.hbm.xml 文件中。下面让我们看看映射文档相关的一些小细节：  

- 映射文档是一个 XML 格式的文档，它拥有 <hibernate-mapping> 作为根元素，这个元素包含了所有的 <class> 元素。  
- **<class>** 元素被用来定义从 Java 类到数据库表的特定的映射。Java 类的名称是特定的，它使用的是类元素的 **name** 属性，数据库表的名称也是特定的，它使用的是 **table** 属性。  
- **<meta>** 元素是一个可选元素它可以用来创建类的描述。  
- **<id>** 元素向数据库的主要关键字表映射类中的特定的 ID 属性。id 元素的 **name** 属性涉及到了类中的属性同时 **column** 属性涉及到了数据库表中的列。**type** 属性掌握了 hibernate 的映射类型，这种映射类型将会从 Java 转到 SQL 数据类型。  
- id 元素中的 **<generator>** 元素是用来自动产生主要关键字的值的。将 generator 元素的 **class** 属性设置成 **native** 从而使 hibernate 运用 **identity**, **sequence** 或者 **hilo** 算法依靠基础数据库的性能来创建主要关键字。  
- **<property>** 元素是用来映射一个 Java 类的属性到数据库的表中的列中。这个元素的  **name** 属性涉及到类中的属性，**column** 属性涉及到数据表中的列。**type** 属性控制  hibernate 的映射类型，这种映射类型将会从 Java 转到 SQL 数据类型。  

映射文档中还应用有许多其他的属性和元素，在探讨其他的 Hibernate 相关的话题时我将会详细进行讲解。  

## 创建应用程序类： 

最后，我们将要使用 main() 方法创建应用程序类来运行应用程序。我们将应用这个应用程序来保存一些 Employee 的记录，然后我们将在这些记录上应用 CRUD 操作。  

```
import java.util.List; 
import java.util.Date;
import java.util.Iterator; 
 
import org.hibernate.HibernateException; 
import org.hibernate.Session; 
import org.hibernate.Transaction;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class ManageEmployee {
   private static SessionFactory factory; 
   public static void main(String[] args) {
      try{
         factory = new Configuration().configure().buildSessionFactory();
      }catch (Throwable ex) { 
         System.err.println("Failed to create sessionFactory object." + ex);
         throw new ExceptionInInitializerError(ex); 
      }
      ManageEmployee ME = new ManageEmployee();

      /* Add few employee records in database */
      Integer empID1 = ME.addEmployee("Zara", "Ali", 1000);
      Integer empID2 = ME.addEmployee("Daisy", "Das", 5000);
      Integer empID3 = ME.addEmployee("John", "Paul", 10000);

      /* List down all the employees */
      ME.listEmployees();

      /* Update employee's records */
      ME.updateEmployee(empID1, 5000);

      /* Delete an employee from the database */
      ME.deleteEmployee(empID2);

      /* List down new list of the employees */
      ME.listEmployees();
   }
   /* Method to CREATE an employee in the database */
   public Integer addEmployee(String fname, String lname, int salary){
      Session session = factory.openSession();
      Transaction tx = null;
      Integer employeeID = null;
      try{
         tx = session.beginTransaction();
         Employee employee = new Employee(fname, lname, salary);
         employeeID = (Integer) session.save(employee); 
         tx.commit();
      }catch (HibernateException e) {
         if (tx!=null) tx.rollback();
         e.printStackTrace(); 
      }finally {
         session.close(); 
      }
      return employeeID;
   }
   /* Method to  READ all the employees */
   public void listEmployees( ){
      Session session = factory.openSession();
      Transaction tx = null;
      try{
         tx = session.beginTransaction();
         List employees = session.createQuery("FROM Employee").list(); 
         for (Iterator iterator = 
                           employees.iterator(); iterator.hasNext();){
            Employee employee = (Employee) iterator.next(); 
            System.out.print("First Name: " + employee.getFirstName()); 
            System.out.print("  Last Name: " + employee.getLastName()); 
            System.out.println("  Salary: " + employee.getSalary()); 
         }
         tx.commit();
      }catch (HibernateException e) {
         if (tx!=null) tx.rollback();
         e.printStackTrace(); 
      }finally {
         session.close(); 
      }
   }
   /* Method to UPDATE salary for an employee */
   public void updateEmployee(Integer EmployeeID, int salary ){
      Session session = factory.openSession();
      Transaction tx = null;
      try{
         tx = session.beginTransaction();
         Employee employee = 
                    (Employee)session.get(Employee.class, EmployeeID); 
         employee.setSalary( salary );
		 session.update(employee); 
         tx.commit();
      }catch (HibernateException e) {
         if (tx!=null) tx.rollback();
         e.printStackTrace(); 
      }finally {
         session.close(); 
      }
   }
   /* Method to DELETE an employee from the records */
   public void deleteEmployee(Integer EmployeeID){
      Session session = factory.openSession();
      Transaction tx = null;
      try{
         tx = session.beginTransaction();
         Employee employee = 
                   (Employee)session.get(Employee.class, EmployeeID); 
         session.delete(employee); 
         tx.commit();
      }catch (HibernateException e) {
         if (tx!=null) tx.rollback();
         e.printStackTrace(); 
      }finally {
         session.close(); 
      }
   }
}
```  

## 编译和执行： 

下面是编译和运行上述提到的应用程序的步骤。在编译和执行应用程序之前确保你已经设置好了 PATH 和 CLASSPATH。  

- 创建设置章节中所讲的 hibernate.cfg.xml 配置文件。  
- 创建上文所述的 Employee.hbm.xml 映射文件。  
- 创建上文所述的 Employee.java 源文件并且进行编译。  
- 创建上文所述的 ManageEmployee.java 源文件并且进行编译。  
- 执行二进制的 ManageEmployee 来运行程序。  

你将会得到如下结果，记录将会在 EMPLOYEE 表中建立。  

```
$java ManageEmployee
.......VARIOUS LOG MESSAGES WILL DISPLAY HERE........

First Name: Zara  Last Name: Ali  Salary: 1000
First Name: Daisy  Last Name: Das  Salary: 5000
First Name: John  Last Name: Paul  Salary: 10000
First Name: Zara  Last Name: Ali  Salary: 5000
First Name: John  Last Name: Paul  Salary: 10000
```  

如果你检查你的 EMPLOYEE 表，它将会有如下记录：  

```
mysql> select * from EMPLOYEE;
+----+------------+-----------+--------+
| id | first_name | last_name | salary |
+----+------------+-----------+--------+
| 29 | Zara       | Ali       |   5000 |
| 31 | John       | Paul      |  10000 |
+----+------------+-----------+--------+
2 rows in set (0.00 sec

mysql>
```  
 
