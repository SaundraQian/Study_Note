---
description: Algonquin College - CST8288 Note
---

# Intro to JDBC

## Establish Connection

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



