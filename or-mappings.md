# O/R 映射

目前为止我们已经通过应用 Hibernate 见识过十分基础的 O/R 映射了，但是还有三个更加重要的有关映射的话题需要我们更详细的探讨。这三个话题是集合的映射，实体类之间的关联映射以及组件映射。  

## 集合映射

如果一个实例或者类中有特定变量的值的集合，那么我们可以应用 Java 中的任何的可用的接口来映射这些值。Hibernate 可以保存 **java.util.Map, java.util.Set, java.util.SortedMap, java.util.SortedSet, java.util.List** 和其它持续的实例或者值的任何**数组**的实例。  

| **集合类型**| **映射和描述** |   
|:---------|:------------|  
|**java.util.Set**|它和 \<set\> 元素匹配并且用 java.util.HashSet 初始化。|  
|**java.util.SortedSet**|它和 \<set\> 元素匹配并且用 java.util.TreeSet 初始化。**sort** 属性可以设置成比较器或者自然排序。|  
|**java.util.List**|它和 \<list\> 元素匹配并且用 java.util.ArrayList 初始化。|  
|**java.util.Collection**|它和 \<bag\> 或者 \<ibag\> 元素匹配以及用 java.util.ArrayList 初始化。|  
|**java.util.Map**|它和 \<map\> 元素匹配并且用 java.util.HashMap 初始化。|  
|**java.util.SortedMap**")|它和 \<map\> 元素匹配并且用 java.util.TreeMap 初始化。**sort** 属性可以设置成比较器或者 自然排序。|  

对于 Java 的原始数值 Hibernate 采用`<primitive-array>`支持数组，对于 Java 的其它数值 Hibernate 采用`<array>`支持数组。然而它们很少被应用，因此我也就不在本指导中讨论它们。  

如果你想要映射一个用户定义的集合接口而这个接口不是 Hibernate 直接支持的话，那么你需要告诉 Hibernate 你定义的这个集合的语法，这个很难操作而且不推荐使用。  

## 关联映射

实体类之间的关联映射以及表之间的关系是 ORM 的灵魂之处。对象间的关系的子集可以用下列四种方式解释。关联映射可以是单向的也可以是双向的。  

| **映射类型**| **描述** |   
|:---------|:------------|  
|**Many-to-One**|使用 Hibernate 映射多对一关系|  
|**One-to-One**|使用 Hibernate 映射一对一关系|  
|**One-to-Many**|使用 Hibernate 映射一对多关系|  
|**Many-to-Many**|使用 Hibernate 映射多对多关系|  

## 组件映射

作为变量的一员实体类很可能和其它类具有相关关系。如果引用的类没有自己的生命周期并且完全依靠于拥有它的那个实体类的生命周期的话，那么这个引用类因此就可以叫做组件类。  

组件集合的映射很可能和正常集合的映射相似，只会有很少的设置上的不同。我们可以在例子中看看这两种映射。  

| **映射类型**| **描述** |   
|:--------|:------------|  
|**Component Mappings**|类的映射对于作为变量的一员的另外的类具有参考作用。| 