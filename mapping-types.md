# 映射类型

当你准备一个 Hibernate 映射文件时，我们已经看到你把 Java 数据类型映射到了 RDBMS 数据格式。在映射文件中已经声明被使用的 **types** 不是 Java 数据类型；它们也不是 SQL 数据库类型。这种类型被称为 Hibernate 映射类型，可以从 Java 翻译成 SQL，反之亦然。  

在这一章中列举出所有的基础，日期和时间，大型数据对象，和其它内嵌的映射数据类型。  

## 原始类型

|映射类型   | Java 类型   | ANSI SQL 类型    |
|:---------|:------------|:------------|
|integer|int 或 java.lang.Integer|INTEGER|
|long|long 或 java.lang.Long	|BIGINT|
|short|short 或 java.lang.Short|SMALLINT|
|float|float 或 java.lang.Float|FLOAT|
|double|double 或 java.lang.Double|DOUBLE|
|big_decimal|java.math.BigDecimal|NUMERIC|
|character|java.lang.String|CHAR(1)|
|string|java.lang.String|VARCHAR|
|byte|byte 或 java.lang.Byte|TINYINT|
|boolean|boolean 或 java.lang.Boolean|BIT|
|yes/no|boolean 或 java.lang.Boolean|CHAR(1) ('Y' or 'N')|
|true/false|boolean 或 java.lang.Boolean|CHAR(1) ('T' or 'F')|

## 日期和时间类型

|映射类型   | Java 类型   | ANSI SQL 类型    |
|:---------|:------------|:------------|
|date|java.util.Date 或 java.sql.Date|DATE|
|time|java.util.Date 或 java.sql.Time|TIME|
|timestamp|java.util.Date 或 java.sql.Timestamp|TIMESTAMP|
|calendar|java.util.Calendar|TIMESTAMP|
|calendar_date|java.util.Calendar|DATE|

## 二进制和大型数据对象

|映射类型   | Java 类型   | ANSI SQL 类型    |
|:---------|:------------|:------------|
|binary|byte[]|VARBINARY (or BLOB)|
|text|java.lang.String|CLOB|
|serializable|any Java class that implements java.io.Serializable|VARBINARY (or BLOB)|
|clob|java.sql.Clob|CLOB|
|blob|java.sql.Blob|BLOB|

## JDK 相关类型

|映射类型   | Java 类型   | ANSI SQL 类型    |
|:---------|:------------|:------------|
|class|java.lang.Class|VARCHAR|
|locale|java.util.Locale|VARCHAR|
|timezone|java.util.TimeZone|VARCHAR|
|currency|java.util.Currency|VARCHAR|