# Hibernate--环境

这个章节会告诉你为了给 Hibernate 应用准备需要的开发环境，该怎样安装 Hibernate 应用和一些其它相关的包。我们会用 MYSQL 数据库来对一些 Hibernate 应用的例子进行试验，所以先要确保你已经安装过了 MYSQL 数据库。想了解更多的关于 MYSQL 数据库的详情的话，你可以搜索我们的[MYSQL教程](http://www.tutorialspoint.com/mysql/index.htm)。
## 下载 Hibernate

如果你已经在你的机器上安装了 Java 的最新版本，那么按照以下这些简易的步骤来下载并安装 Hibernate 在你的机器上就可以了。    

- 首先要在想要把 Hibernate 应用安装在 Windows 系统或是 Unix 系统这两者之间做出选择，之后继续到下一个步骤去下载与 Windows 系统对应的 .zip 文件或是与 Unix 系统对应的 .tz文件。
  
- 之后到 [http://www.hibernate.org/downloads](http://www.hibernate.org/downloads) 这个网址来下载最新版本的 Hibernate 应用。
      
- 在写这个教程时我下载的是 **hibernate-distribution-3.6.4.Final** 这个版本的应用，在这个版本的应用下当我们解压下载的文件时会显示以下的目录结构。  
![图片](http://www.tutorialspoint.com/hibernate/images/hibernate_directories.jpg)    
## 安装 Hibernate

一旦你下好并且解压了 Hibernate 应用最新版本的安装文件，你需要执行以下两个简单的步骤。一定要确保你把你的 CLASSPATH 变量设置的合理了，否则当你编译你的应用时可能会遇到问题。

- 首先把从 **/lib** 复制来的所有库文件拷贝到 CLASSPATH 里，并且改变你的 CLASSPATH 变量来涵盖所有的 JAR。

- 最后复制 **hibernate3.jar** 这个文件到 CLASSPATH 里。这个文件位于安装文件的根目录里，它是Hibernate 应用针对的主要 JAR。
## 安装 Hibernate 的条件

以下是一个 Hibernate 应用需要的有关包/库的表格，在安装 Hibernate 应用之前你需要先安装它们。为了安装这些包你必须把来自 **/lib** 的库文件拷贝到 CLASSPATH ，并按以下说明相应地改变 CLASSPATH 变量。

**S.N.**	**包/库**  
1.	**dom4j** - XML parsing [www.dom4j.org/](http://www.dom4j.org/)  
2.	**Xalan** - XSLT Processor [http://xml.apache.org/xalan-j/](http://xml.apache.org/xalan-j/)  
3.	**Xerces** - The Xerces Java Parser [http://xml.apache.org/xerces-j/](http://xml.apache.org/xerces-j/)   
4.	**cglib** - Appropriate changes to Java classes at runtime[http://cglib.sourceforge.net/](http://cglib.sourceforge.net/)   
5.	**log4j** - Logging Faremwork [http://logging.apache.org/log4j](http://logging.apache.org/log4j)    
6.	**Commons** - Logging, Email etc. [http://jakarta.apache.org/commons](http://jakarta.apache.org/commons)    
7.	**SLF4J** - Logging Facade for Java [http://www.slf4j.org](http://www.slf4j.org)  
 
  
