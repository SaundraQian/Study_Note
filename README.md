---
description: Algonquin College - CST8288 Note
---

# Intro to JDBC

ddEstablish Connection

### DriverManager

DriverManager is responsible for driver management, while the database driver is for application services. Getting connection through DriverManager is a very important thing for application developers.&#x20;

Good tutorial: [https://www.tutorialspoint.com/jdbc/index.htm](https://www.tutorialspoint.com/jdbc/index.htm)

* `getConnection(...)` creates instances of Connection

```java
// establish connection to database                              
connection = DriverManager.getConnection(
                url, username, password );

```

* throws `SQLException` and `SQLTimeoutException` to used to describe the exceptions that may occur in database related operations.  more information: [https://cloud.tencent.com/developer/article/1388022](https://cloud.tencent.com/developer/article/1388022)

```java
public static void deregisterDriver(Driver driver) 
    throws SQLException {...}
```

* overloaded method
* common form uses 3 arguments of type String：
  * cinnection String - URL-like String specific for each JDBC driver & can specify options
  * userid & password

<pre class="language-java"><code class="lang-java">//Database connection required parameters
<strong>String user = "root";
</strong>String password = "123456";
String url = "jdbc:mysql://hostname:portNumbner/databaseName";
</code></pre>

* use Properties class to store details&#x20;
* JDBC driver contained in .jar file added to project. Dricers are autoloaded (usually). If not then, load with `Class.forName()` with filly qualified name of drivers class

```java
try {
   Class.forName("com.mysql.cj.jdbc.Driver");
}
catch(ClassNotFoundException ex) {
   System.out.println("Error: unable to load driver class!");
   System.exit(1);
}
```

### DataSource

DataSource is alternative approach which uses similar info to DataManager but JNDI (Java Naming & Directory Interface) is used to lookup the data source info. Therefore, the connection string, etc is stored in JNDI and then queried by name that  means we need a JNDI servers which builtin to some application sercers such as Payara and can be added to TomCat.&#x20;

（CST8288 will not use DataSource)

## Create Statement & Query

### CreateStatement

After we have instance of Connection, we use `createStatement()` method to obtain instance of Statement.

```java
Statement statement = connection.createStatement();
```

It is a overloaded method which common from has no aeguments

### PreparedStatement

`PreparedStatement` like Statement but uses `Statement` is the parent class of `PreparedStatement`

```java
Statement statement = connection.createStatement(sql);
```

more detail: [https://blog.csdn.net/weixin\_41092717/article/details/82853704](https://blog.csdn.net/weixin\_41092717/article/details/82853704)

* CallableStatement used to invoke stored procedure in database (won't use it in this course)

## Execute statement

with an instance of Statement, using overloaded methods:

* ExecuteQuery(): used for SELECT and return ResultSet
* ExecuteUpdate(): for INSERT, UPDATE, DELETE etc

```java
ResultSet rs=stmt.executeQuery("select * from ...");

int rows=stmt.executeUpdate("insert into ...");
```

Simplest form use a single String argument which containing the SQL command.

## Result & Metadata

### ResultSetMetaData

use getMetaData() with instance of ResultSet then use getter to obtain: count of columns, name of a column, data type for column ... and more&#x20;

\*Caution： column numbering starts at 1 （NOT ZERO！！）

More detail: [https://blog.csdn.net/J080624/article/details/53728916](https://blog.csdn.net/J080624/article/details/53728916)

<pre class="language-java"><code class="lang-java">ResultSetMetaData rsmd = resultSet.getMetaData(); 

//Get the name of the specified column
getColumnName(int column)：

//Returns the number of columns 
//in the current ResultSet object.
getColumnCount()： 

<strong>//Retrieves the database specific type name 
</strong><strong>//of the specified column.
</strong>getColumnTypeName(int column)： 

//Indicates the maximum standard width 
//of the specified column, in characters. 
getColumnDisplaySize(int column)：

//Indicates whether the value 
//in the specified column can be null. 
isNullable(int column)：

//Indicates whether the specified columns 
//are automatically numbered so that they remain read-only. 
isAutoIncrement(int column)：
</code></pre>

### ResultSet

getters for each data type which get by column name or column number.&#x20;

to loop through a `ResultSet()` instance by row looks like Iterator from Collecrtion but it is not <mark style="color:purple;">"cursor" is positioned before first row</mark> when `ResultSet()` instance created

also no `hasNext()` method, use `next()` instead

more detail: [https://www.cnblogs.com/qianguyihao/p/4054235.html](https://www.cnblogs.com/qianguyihao/p/4054235.html)

```java
while (some_instance_resultSet.next()){
//process a row using getters for each column
}

//Example
while (resultSet.next()) {
        for (int i = 1; i <= numberOfColumns; i++) {
                System.out.printf("%-8s\t", resultSet.getObject(i));
        }
        System.out.println();
    } // end while

```

`ResultSet()` can retrieve rows by going forward like the previous example or in either direction.

when `ResultSet()` changes, it can also change underlying database immediatedly or wait next query (these teatures set with `createStatement()`

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>Week 3 PPT - p13</p></figcaption></figure>

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption><p>Week 3 PPT - p14</p></figcaption></figure>

## Close Connection, etc

We close in the reverse order using `close()` method

more detail：[https://blog.csdn.net/x\_iya/article/details/70228869](https://blog.csdn.net/x\_iya/article/details/70228869)

```java
Connection conn = null;
PreparedStatement ps = null;
ResultSet rs = null;
 
try {
    ...
} catch (SQLException ex) {
    ...
} finally {
    if (rs != null) {
        try {
            rs.close();
        } catch (SQLException e) { /* ignored */}
    }
    if (ps != null) {
        try {
            ps.close();
        } catch (SQLException e) { /* ignored */}
    }
    if (conn != null) {
        try {
            conn.close();
        } catch (SQLException e) { /* ignored */}
    }
}
```

## Properties Class

subclass of HashTable, which stores a "key" along with its "value" (both are limited to type String). More detail: [https://www.runoob.com/java/java-hashtable-class.html](https://www.runoob.com/java/java-hashtable-class.html)

get & set with overloaded methods `getProperty()`, `setProperty()`. More detail: [https://blog.csdn.net/qq\_36894974/article/details/79050297](https://blog.csdn.net/qq\_36894974/article/details/79050297)

uses a file as a persistent store&#x20;

* read from a file: load()
* write to a file: list()

useful to store JDBC connection, string, userid\&password
