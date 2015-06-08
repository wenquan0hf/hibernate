# 对象关系映射框架 - 持久化类

Hibernate 的完整概念是提取 Java 类属性中的值，并且将它们保存到数据库表单中。映射文当能够帮助 Hibernate 确定如何从该类中提取值，并将它们映射在表格和相关域中。

在 Hibernate 中，其对象或实例将会被存储在数据库表单中的 Java 类被称为持久化类。若该类遵循一些简单的规则或者被大家所熟知的 Plain Old Java Object (POJO) 编程模型，Hibernate 将会处于其最佳运行状态。以下所列就是持久化类的主要规则，然而，在这些规则中，没有一条是很难的要求。

- 所有将被持久化的 Java 类都需要一个默认的构造函数。

- 为允许在 Hibernate 和数据库中对对象的简单识别，所有类都需要包含一个 ID。此属性映射到数据库表的主键列。

- 所有将被持久化的属性都应该声明为 private，并具有由 JavaBean 风格定义的 **getXXX** 和 **setXXX** 方法。

- Hibernate 的一个重要特征为代理，它取决于该持久化类是处于 non-final 的，还是处于在已声明所有公共方法界面的执行状态。

- 所有不扩展或执行一些专门类和 EJB 框架所需的界面的类。

POJO 的名称用于强调一个给定的对象是普通的 Java 对象，而不是特殊的对象，尤其不是一个 Enterprise JavaBean。

## 一个简单的 POJO 的例子：

基于以上所述规则，我们能够定义如下 POLO 类：

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





 
  
