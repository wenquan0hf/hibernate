# 注释

到现在为止，你已经看到 Hibernate 如何使用 XML 映射文件来完成从 POJO 到数据库表的数据转换的，反之亦然。 Hibernate 注释是无需使用 XML 文件来定义映射的最新方法。你可以额外使用注释或直接代替 XML 映射元数据。

Hibernate 注释是一种强大的来给对象和关系映射表提供元数据的方法。所有的元数据被添加到 POJO java 文件代码中，这有利于用户在开发时更好的理解表的结构和 POJO。

如果你想让你的应用程序移植到其它 EJB 3 的 ORM 应用程序中,您必须使用注释来表示映射信息，但是如果想要得到更大的灵活性,那么你应该使用基于 XML 的映射。

## Hibernate 注释的环境设置

首先你必须确定你使用的是 JDK 5.0，否则你需要升级你的 JDK 至 JDK 5.0，来使你的主机能够支持注释。

其次，你需要安装 Hibernate 3.x 注释包，可以从 sourceforge 行下载：[（**下载 Hibernate 注释**）](http://sourceforge.net/projects/hibernate/files/hibernate-annotations/) 并且从 Hibernate 注释发布中拷贝 **hibernate-annotations.jar, lib/hibernate-comons-annotations.jar** 和 **lib/ejb3-persistence.jar** 到你的 CLASSPATH。

## 注释类示例

正如我上面所提到的，所有的元数据被添加到 POJO java 文件代码中，这有利于用户在开发时更好的理解表的结构和 POJO。

下面我们将使用 EMPLOYEE 表来存储对象:

```
create table EMPLOYEE (
   id INT NOT NULL auto_increment,
   first_name VARCHAR(20) default NULL,
   last_name  VARCHAR(20) default NULL,
   salary     INT  default NULL,
   PRIMARY KEY (id)
);
```

以下是用带有注释的 Employee 类来映射使用定义好的 Employee 表的对象:

```
import javax.persistence.*;

@Entity
@Table(name = "EMPLOYEE")
public class Employee {
   @Id @GeneratedValue
   @Column(name = "id")
   private int id;

   @Column(name = "first_name")
   private String firstName;

   @Column(name = "last_name")
   private String lastName;

   @Column(name = "salary")
   private int salary;  

   public Employee() {}
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

Hibernate 检测到 @Id 注释字段并且认定它应该在运行时通过字段直接访问一个对象上的属性。如果你将 @Id 注释放在 getId() 方法中，你可以通过默认的 getter 和 setter 方法来访问属性。因此，所有其它注释也放在字段或是 getter 方法中，决定于选择的策略。下一节将解释上面的类中使用的注释。

## @Entity 注释

EJB 3 标准的注释包含在 **javax.persistence** 包，所以我们第一步需要导入这个包。第二步我们对 Employee 类使用 **@Entity 注释**，标志着这个类为一个实体 bean，所以它必须含有一个没有参数的构造函数并且在可保护范围是可见的。

## @Table 注释

@table 注释允许您明确表的详细信息保证实体在数据库中持续存在。

@table 注释提供了四个属性，允许您覆盖的表的名称，目录及其模式,在表中可以对列制定独特的约束。现在我们使用的是表名为 EMPLOYEE。

## @Id 和 @GeneratedValue 注释

每一个实体 bean 都有一个主键，你在类中可以用 **@Id** 来进行注释。主键可以是一个字段或者是多个字段的组合，这取决于你的表的结构。

默认情况下，@Id 注释将自动确定最合适的主键生成策略，但是你可以通过使用 **@GeneratedValue** 注释来覆盖掉它。**strategy** 和 **generator** 这两个参数我不打算在这里讨论，所以我们只使用默认键生成策略。让 Hibernate 确定使用哪些生成器类型来使代码移植于不同的数据库之间。

## @Column Annotation

@Column 注释用于指定某一列与某一个字段或是属性映射的细节信息。您可以使用下列注释的最常用的属性:

- **name** 属性允许显式地指定列的名称。
- **length** 属性为用于映射一个值，特别为一个字符串值的列的大小。
- **nullable** 属性允许当生成模式时，一个列可以被标记为非空。
- **unique** 属性允许列中只能含有唯一的内容

## 创建应用类

最后,我们将创建应用程序类，并使用 main() 方法来运行应用程序。我们将使用此应用程序来保存一些员工的记录,然后我们对这些记录进行 CRUD 操作。

```
import java.util.List; 
import java.util.Date;
import java.util.Iterator; 
 
import org.hibernate.HibernateException; 
import org.hibernate.Session; 
import org.hibernate.Transaction;
import org.hibernate.cfg.AnnotationConfiguration;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class ManageEmployee {
   private static SessionFactory factory; 
   public static void main(String[] args) {
      try{
         factory = new AnnotationConfiguration().
                   configure().
                   //addPackage("com.xyz") //add package if used.
                   addAnnotatedClass(Employee.class).
                   buildSessionFactory();
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
         Employee employee = new Employee();
         employee.setFirstName(fname);
         employee.setLastName(lname);
         employee.setSalary(salary);
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

## 数据库配置

现在，让我们创建 **hibernate.cfg.xml** 配置文件来定义数据库相关参数。

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

   <!-- Assume students is the database name -->
   <property name="hibernate.connection.url">
      jdbc:mysql://localhost/test
   </property>
   <property name="hibernate.connection.username">
      root
   </property>
   <property name="hibernate.connection.password">
      cohondob
   </property>

</session-factory>
</hibernate-configuration>
```

## 编译和执行

这里是编译并运行以上提到的应用程序的步骤。再继续编译和运行之前需要确保你正确设置路径和类路径。

- 从目录中删除 Employee.hbm.xml 映射文件。
- 创建上述 Employee.java 源文件并编译。
- 创建上述 ManageEmployee.java 源文件并编译。
- 执行 ManageEmployee 二进制程序。

你将得到如下结果，并且会在 EMPLOYEE 表中记录。

```
$java ManageEmployee
.......VARIOUS LOG MESSAGES WILL DISPLAY HERE........

First Name: Zara  Last Name: Ali  Salary: 1000
First Name: Daisy  Last Name: Das  Salary: 5000
First Name: John  Last Name: Paul  Salary: 10000
First Name: Zara  Last Name: Ali  Salary: 5000
First Name: John  Last Name: Paul  Salary: 10000
```

如果你查看 EMPLOYEE 表，它将有如下记录：

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