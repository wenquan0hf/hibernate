# 对象关系映射框架 - 会话

Session 用于获取与数据库的物理连接。 Session 对象是轻量级的，并且设计为在每次需要与数据库进行互动时被实例化。持久态对象被保存，并通过 Session 对象检索找回。

该 session 对象不应该长时间保持开放状态，因为它们通常不能保持线程安全，而应该根据需求被创建和销毁。Session 的主要功能是为映射实体类的实例提供创建，读取和删除操作。这些实例可能在给定时间点时存在于以下三种状态之一：

- **瞬时状态**: 一种新的持久性实例，被 Hibernate 认为是瞬时的，它不与 Session 相关联，在数据库中没有与之关联的记录且无标识符值。

- **持久状态**：可以将一个瞬时状态实例通过与一个 Session 关联的方式将其转化为持久状态实例。持久状态实例在数据库中没有与之关联的记录，有标识符值，并与一个 Session 关联。

- **脱管状态**：一旦关闭 Hibernate Session，持久状态实例将会成为脱管状态实例。

若 Session 实例的持久态类别是序列化的，则该 Session 实例是序列化的。一个典型的事务应该使用以下习语：

```
Session session = factory.openSession();
Transaction tx = null;
try {
   tx = session.beginTransaction();
   // do some work
   ...
   tx.commit();
}
catch (Exception e) {
   if (tx!=null) tx.rollback();
   e.printStackTrace(); 
}finally {
   session.close();
}
```

如果 Session 引发异常，则事务必须被退回，该 session 必须被丢弃。

## Session 界面方法：

**Session** 界面提供了很多方法，但在以下讲解中我将仅列出几个我们会在本教程中应用的重要方法。您可以查看 Hibernate 文件，查询与 **Session** 及**SessionFactory** 相关的完整方法目录。

|序号|Session 方法及说明|
|----|:-----------------|
|1|**Transaction beginTransaction()**</br>开始工作单位，并返回关联事务对象。|
|2|**void cancelQuery()**</br>取消当前的查询执行。|
|3|**void clear()**</br>完全清除该 session。|
|4|**Connection close()**</br>通过释放和清理 JDBC 连接结束该 session。|
|5|**Criteria createCriteria(Class persistentClass)**</br>为给定的实体类或实体类的超类创建一个新的 Criteria 实例。|
|6|**Criteria createCriteria(String entityName)**</br>为给定的实体名称创建一个新的 Criteria 实例。|
|7|**Serializable getIdentifier(Object object)**</br>返回与给定实体相关联的 session 的标识符值。|
|8|**Query createFilter(Object collection, String queryString)**</br>为给定的集合和过滤字符创建查询的新实例。|
|9|**Query createQuery(String queryString)**</br>为给定的 HQL 查询字符创建查询的新实例。|
|10|**SQLQuery createSQLQuery(String queryString)**</br>为给定的 SQL 查询字符串创建 SQLQuery 的新实例。|
|11|**void delete(Object object)**</br>从数据存储中删除持久化实例。|
|12|**void delete(String entityName, Object object)**</br>从数据存储中删除持久化实例。|
|13|**Session get(String entityName, Serializable id)**</br>返回给定命名的且带有给定标识符或 null 的持久化实例（若无该种持久化实例）。|
|14|**SessionFactory getSessionFactory()**</br>获取创建该 session 的 session 工厂。|
|15|**void refresh(Object object)**</br>从基本数据库中重新读取给定实例的状态。|
|16|**Transaction getTransaction()**</br>获取与该 session 关联的事务实例。|
|17|**boolean isConnected()**</br>检查当前 session 是否连接。|
|18|**boolean isDirty()**</br>该 session 中是否包含必须与数据库同步的变化？|
|19|**boolean isOpen()**</br>检查该 session 是否仍处于开启状态。|
|20|**Serializable save(Object object)**</br>先分配一个生成的标识，以保持给定的瞬时状态实例。|
|21|**void saveOrUpdate(Object object)**</br>保存（对象）或更新（对象）给定的实例。|
|22|**void update(Object object)**</br>更新带有标识符且是给定的处于脱管状态的实例的持久化实例。|
|23|**void update(String entityName, Object object)**</br>更新带有标识符且是给定的处于脱管状态的实例的持久化实例。|

 
  
