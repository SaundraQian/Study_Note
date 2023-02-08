# Intro to JDBC

## DriverManager

DriverManager is responsible for driver management, while the database driver is for application services. Getting connection through DriverManager is a very important thing for application developers.

* getConnection(...) creates instances of Connection

```java
// establish connection to database                              
connection = DriverManager.getConnection(
                url, username, password );

```
