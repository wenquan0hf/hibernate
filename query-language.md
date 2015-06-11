# Hibernate - 查询语言

Hibernate 查询语言 （HQL） 是一种面向对象的查询语言，类似于SQL，但是不是去对表和列进行操作，而是面向对象和他们的属性。 HQL 查询被 Hibernate 翻译为传统的 SQL 查询从而对数据库进行操作。

尽管你能直接使用本地 SQL 语句，但我还是建议你尽可能的使用 HQL 语句，以避免数据库关于可移植性的麻烦，并且体现了 Hibernate 的 SQL 生成和缓存策略。

在 HQL 中一些关键字比如 SELECT ，FROM 和 WHERE 等，是不区分大小写的，但是一些属性比如表名和列名是区分大小写的。

## FROM 从句

如果你想要在存储中加载一个完整并持续的对象,你将使用 **FROM** 子句。以下是 FROM 子句的一些简单的语法：

```
String hql = "FROM Employee";
Query query = session.createQuery(hql);
List results = query.list();
```

如果你需要在 HQL 中完全限定类名，只需要指定包和类名，如下：

```
String hql = "FROM com.hibernatebook.criteria.Employee";
Query query = session.createQuery(hql);
List results = query.list();
```

## AS 子句

在 HQL 中 **AS** 子句能够用来给你的类分配别名，尤其是在长查询的情况下。例如，我们之前的例子，可以用如下方式展示：

```
String hql = "FROM Employee AS E";
Query query = session.createQuery(hql);
List results = query.list();
```

关键字 **AS** 是可选择的并且你也可以在类名后直接指定一个别名，如下：

```
String hql = "FROM Employee E";
Query query = session.createQuery(hql);
List results = query.list();
```

## SELECT 子句

**SELECT** 子句比 from 子句提供了更多的对结果集的控制。如果你只想得到对象的几个属性而不是整个对象你需要使用 SELECT 子句。下面是一个 SELECT 子句的简单语法示例，这个例子是为了得到 Employee 对象的 first_name 字段：

```
String hql = "SELECT E.firstName FROM Employee E";
Query query = session.createQuery(hql);
List results = query.list();
```

值得注意的是 **Employee.firstName** 是 Employee 对象的属性，而不是一个表的字段。

## WHERE 子句

如果你想要精确的从数据库存储中返回特定对象，你需要使用 WHERE 子句。下面是 WHERE 子句的简单语法例子：

```
String hql = "FROM Employee E WHERE E.id = 10";
Query query = session.createQuery(hql);
List results = query.list();
```

## ORDER BY 子句

为了给 HSQ 查询结果进行排序，你将需要使用 **ORDER BY** 子句。你能利用任意一个属性给你的结果进行排序，包括升序或降序排序。下面是一个使用 ORDER BY 子句的简单示例：

```
String hql = "FROM Employee E WHERE E.id > 10 ORDER BY E.salary DESC";
Query query = session.createQuery(hql);
List results = query.list();
```

如果你想要给多个属性进行排序，你只需要在 ORDER BY 子句后面添加你要进行排序的属性即可，并且用逗号进行分割：

```
String hql = "FROM Employee E WHERE E.id > 10 " +
             "ORDER BY E.firstName DESC, E.salary DESC ";
Query query = session.createQuery(hql);
List results = query.list();
```

## GROUP BY 子句

这一子句允许 Hibernate 将信息从数据库中提取出来，并且基于某种属性的值将信息进行编组,通常而言,该子句会使用得到的结果来包含一个聚合值。下面是一个简单的使用 GROUP BY 子句的语法:

```
String hql = "SELECT SUM(E.salary), E.firtName FROM Employee E " +
             "GROUP BY E.firstName";
Query query = session.createQuery(hql);
List results = query.list();
```

## 使用命名参数

Hibernate 的 HQL 查询功能支持命名参数。这使得 HQL 查询功能既能接受来自用户的简单输入，又无需防御 SQL 注入攻击。下面是使用命名参数的简单的语法:

```
String hql = "FROM Employee E WHERE E.id = :employee_id";
Query query = session.createQuery(hql);
query.setParameter("employee_id",10);
List results = query.list();
```

## UPDATE 子句

HQL Hibernate 3 较 HQL Hibernate 2，新增了批量更新功能和选择性删除工作的功能。查询接口包含一个 executeUpdate() 方法，可以执行 HQL 的 UPDATE 或 DELETE 语句。

**UPDATE** 子句能够更新一个或多个对象的一个或多个属性。下面是使用 UPDATE 子句的简单的语法:

```
String hql = "UPDATE Employee set salary = :salary "  + 
             "WHERE id = :employee_id";
Query query = session.createQuery(hql);
query.setParameter("salary", 1000);
query.setParameter("employee_id", 10);
int result = query.executeUpdate();
System.out.println("Rows affected: " + result);
```

## DELETE 子句

**DELETE** 子句可以用来删除一个或多个对象。以下是使用 DELETE 子句的简单语法：

```
String hql = "DELETE FROM Employee "  + 
             "WHERE id = :employee_id";
Query query = session.createQuery(hql);
query.setParameter("employee_id", 10);
int result = query.executeUpdate();
System.out.println("Rows affected: " + result);
```

## INSERT 子句

HQL 只有当记录从一个对象插入到另一个对象时才支持 **INSERT INTO** 子句。下面是使用 INSERT INTO 子句的简单的语法:

```
String hql = "INSERT INTO Employee(firstName, lastName, salary)"  + 
             "SELECT firstName, lastName, salary FROM old_employee";
Query query = session.createQuery(hql);
int result = query.executeUpdate();
System.out.println("Rows affected: " + result);
```

## 聚合方法

HQL 类似于 SQL，支持一系列的聚合方法,他们以同样的方式在 HQL 和 SQL 中工作，以下列出了几种可用方法：

|S.N.|方法|描述|
| ------------- |:-------------:|:-------------:| 
|1|avg(property name)|属性的平均值|
|2|count(property name or *)|属性在结果中出现的次数|
|3|max(property name)|属性值的最大值|
|4|min(property name)|属性值的最小值|
|5|sum(property name)|属性值的总和|

**distinct** 关键字表示只计算行集中的唯一值。下面的查询只计算唯一的值:

```
String hql = "SELECT count(distinct E.firstName) FROM Employee E";
Query query = session.createQuery(hql);
List results = query.list();
```

## 使用分页查询

以下为两种分页查询界面的方法：

|S.N.|方法&描述|
| ------------- |:-------------:|
|1|Query setFirstResult(int startPosition) <br> 该方法以一个整数表示结果中的第一行,从0行开始。|
|2|Query setMaxResults(int maxResult) <br> 这个方法告诉 Hibernate 来检索固定数量，即 **maxResults** 个对象。|

使用以上两种方法，我们可以在我们的 web 或 Swing 应用程序中构造一个分页组件。下面是示例,您可以扩展到每次取 10 行:

```
String hql = "FROM Employee";
Query query = session.createQuery(hql);
query.setFirstResult(1);
query.setMaxResults(10);
List results = query.list();
```
 
  
