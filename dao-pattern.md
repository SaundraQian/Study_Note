---
description: Algonquin College CST8288 note
---

# DAO pattern

## Data Access Object (DAO）

Data Access Object sparetes business/domain from data layer: abstract & encapsulates data source & access

* CRUD functions: create, Retrieve, Update and Delete

motiveation of DAO is to isolate business/domain from difference amoung persistence implementations. For example:

* different SQL RDBMS implementations
* SQL vs non-SQL persistence
* RDMBS vs directories (such as LDAP, ActiveDirectory)

Factory pattern Commonly added (a DAO factory)

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption><p>Week 5 PPT - P4</p></figcaption></figure>

## DAO - typical components

Data Source&#x20;

* persistence implemention such as Object DBMDS, RDBMS, LDAP, XML, flat file.
* inclucde main method

````java
import java.io.IOException;
import java.io.InputStream;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;

public class DataSource {
    
    private Connection connection = null;
	
    public Connection createConnection() {
        
        Properties props = new Properties();

        try(InputStream input = Files.newInputStream(Paths.get("src/Database.properties"))){
            props.load(input);
        }catch(IOException e){
            e.printStackTrace();
        }

        String url = props.getProperty("jdbc.url");
        String user = props.getProperty("jdbc.user");
        String password = props.getProperty("jdbc.password");

        try{
           if (connection != null){
               System.out.println("Connection is already established");
            } else {
               connection = DriverManager.getConnection(url, user, password);
            }
        }catch (SQLException e){
            e.printStackTrace();
        }

        return connection;
    }
}
```
````

Domain or Business Object, Client, Context

* requires access to Data Dources to obtain/store data
* implements all the abstract methods in the interface

Data Access Object (DAO)

* CRUD operations for Domain Object
* abstracts access to Data Source which implementation can change w/o impact on Domain layer
* Interface

```java
import PersonDTO;
import java.util.ArrayList;
import java.util.List;
import java.sql.PreparedStatement;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;


public class PersonDaoImpl implements PersonDao {

    @Override
    public List<PersonDTO> getAllPersons() {
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        ArrayList<PersonDTO> persons = null;

        try {
            DataSource ds = new DataSource();
            conn = ds.createConnection();
            ps = conn.prepareStatement(
                "SELECT idAwardID, name, year, city, category FROM person order by idAwardID");
            rs = ps.executeQuery();
        
            persons = new ArrayList<PersonDTO>();
            while (rs.next()) {
                PersonDTO person = new PersonDTO();
                person.setIdAwardID(rs.getInt("idAwardID"));
                person.setName(rs.getString("name"));
                person.setYear(rs.getInt("year"));
                person.setCity(rs.getString("city"));
                person.setCategory(rs.getString("category"));
                persons.add(person);
            }
            
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (rs != null) {
                    rs.close();
                }
            } catch (SQLException e) {
            System.out.println(e.getMessage());
            }
            try {
                if (ps != null) {
                ps.close();
                }
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
            try {
                if (conn != null) {
                conn.close();
                }
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
        }
        
        return persons;
    }

    @Override
    public list<PersonDTO> getAllMetaData() {
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        ArrayList<PersonDTO> metaData = null;
        ResultSetMetaData rsmd = null;

        try {
            DataSource ds = new DataSource();
            conn = ds.createConnection();
            ps = conn.prepareStatement(
                "SELECT idAwardID, name, year, city, category FROM person order by idAwardID");
            rs = ps.executeQuery();
            rsmd = rs.getMetaData();
        
            metaData = new ArrayList<PersonDTO>();
            while (rs.next()) {
                PersonDTO person = new PersonDTO();
                person.setColumnClassName(rsmd.getColumnName(i));
                person.setColumnType(rsmd.getColumnType(i));
                person.setColumnTypeName(rsmd.getColumnTypeName(i));
                person.add(person);
            }
            
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (rs != null) {
                    rs.close();
                }
            } catch (SQLException e) {
            System.out.println(e.getMessage());
            }
            try {
                if (ps != null) {
                ps.close();
                }
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
            try {
                if (conn != null) {
                conn.close();
                }
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
        }

        return metaData;
    }

    @Override
    public PersonDTO getPersonById(int idAwardID) {
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        PersonDTO person = null;

        try {
            DataSource ds = new DataSource();
            conn = ds.createConnection();
            ps = conn.prepareStatement(
                "SELECT idAwardID, name, year, city, category FROM person WHERE idAwardID = ?");
            ps.setInt(1, idAwardID);
            rs = ps.executeQuery();
        
            if (rs.next()) {
                person = new PersonDTO();
                person.setIdAwardID(rs.getInt("idAwardID"));
                person.setName(rs.getString("name"));
                person.setYear(rs.getInt("year"));
                person.setCity(rs.getString("city"));
                person.setCategory(rs.getString("category"));
            }
            
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (rs != null) {
                    rs.close();
                }
            } catch (SQLException e) {
            System.out.println(e.getMessage());
            }
            try {
                if (ps != null) {
                ps.close();
                }
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
            try {
                if (conn != null) {
                conn.close();
                }
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
        }
        
        return person;
    }

    @Override
    public void insertPerson(PersonDTO person) {
        Connection conn = null;
        PreparedStatement ps = null;

        try {
            DataSource ds = new DataSource();
            conn = ds.createConnection();
            ps = conn.prepareStatement(
                "INSERT INTO person (idAwardID, name, year, city, category) VALUES (?, ?, ?, ?, ?)");
            ps.setInt(1, person.getIdAwardID());
            ps.setString(2, person.getName());
            ps.setInt(3, person.getYear());
            ps.setString(4, person.getCity());
            ps.setString(5, person.getCategory());
            ps.executeUpdate();
            
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (ps != null) {
                ps.close();
                }
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
            try {
                if (conn != null) {
                conn.close();
                }
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
        }
    }

    @Override
    public void updatePerson(PersonDTO person) {
        Connection conn = null;
        PreparedStatement ps = null;

        try {
            DataSource ds = new DataSource();
            conn = ds.createConnection();
            ps = conn.prepareStatement(
                "UPDATE person SET name = ?, year = ?, city = ?, category = ? WHERE idAwardID = ?");
            ps.setString(1, person.getName());
            ps.setInt(2, person.getYear());
            ps.setString(3, person.getCity());
            ps.setString(4, person.getCategory());
            ps.setInt(5, person.getIdAwardID());
            ps.executeUpdate();
            
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (ps != null) {
                ps.close();
                }
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
            try {
                if (conn != null) {
                conn.close();
                }
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
        }
    }

    @Override   
    public void deletePerson(int idAwardID) {
        Connection conn = null;
        PreparedStatement ps = null;

        try {
            DataSource ds = new DataSource();
            conn = ds.createConnection();
            ps = conn.prepareStatement(
                "DELETE FROM person WHERE idAwardID = ?");
            ps.setInt(1, idAwardID);
            ps.executeUpdate();
            
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (ps != null) {
                ps.close();
                }
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
            try {
                if (conn != null) {
                conn.close();
                }
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
        }
    }
}
```

Data Transfer Object (DTO) or Value Object

* encapsulates data sent between DAO and Domain layer and foole Java Bean conventions&#x20;
* get/set method

````java
public class PersonDTO {
    
    private int idAwardID;
    private String name;
    private int year;
    private String city;
    private String category;
    private String columnName;
    private String columnTypeName;
    private String columnClassName;
   
    public Integer getIdAwardID() {
        return idAwardID;
    }
    
    public void setIdAwardID(int idAwardID) {
        this.idAwardID = idAwardID;
    }
    
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
    
    public int getYear() {
        return year;
    }
    
    public void setYear(int year) {
        this.year = year;
    }

    public String getCity() {
        return city;
    }
    
    public void setCity(String city) {
        this.city = city;
    }
    
    public String getCategory() {
        return category;
    }
    
    public void setCategory(String category) {
        this.category = category;
    }

    public String getColumnName() {
        return columnName;
    }
   
    public void setColumnName(String columnName) {
        this.columnName = columnName;
    }

    public String getColumnTypeName() {
        return columnTypeName;
    }
    
    public void setColumnTypeName(String columnTypeName) {
        this.columnTypeName = columnTypeName;
    }
    
    public String getColumnClassName() {
        return columnClassName;
    }
    
    public void setColumnClassName(String columnClassName) {
        this.columnClassName = columnClassName;
    }
}

```
````

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption><p>Week 4 PPT - P6</p></figcaption></figure>

More Detail and code example：[https://blog.csdn.net/weixin\_43621681/article/details/89307290](https://blog.csdn.net/weixin\_43621681/article/details/89307290)

## DAO - optional components

DAO interface

* often used with  mult. entites in Data Source
  * same interface to acess all entities
  * a concrete DAO class created for each entity
  * naming conventaion: entity\_nameDAO

```java
public interface PersonDao {

   list<PersonDTO> getAllPersons();

   list<PersonDTO> getAllMetaData();

    PersonDTO getPersonById(int idAwardID);

    void add(PersonDTO person);

    void update(PersonDTO person);

    void delete(int idAwardID);   
}

```

* DAO exceptions
  * Domain Object should not get "raw" exceptions from DATA Source (e.g. SQLException)
  * DAO should handle exception related to Data Source and throw custom exceptions to Damain Object tha are netural/independent of Data source
  * more detail: [https://blog.csdn.net/qq\_40178464/article/details/81639456](https://blog.csdn.net/qq\_40178464/article/details/81639456)

````java
public class ValidationException extends Exception {

    
public ValidationException() {
        super("Validation error");
    }

    public ValidationException(String message) {
        super(message);
    }
    
    public ValidationException(String message, Throwable cause) {
        super(message, cause);
    }

    public ValidationException(Throwable cause) {
        super(cause);
    }

    
}
```
````

## Value Object, DTO and Dependent Object

Value Object: small object that encapsulates a value and typically immutable e.g. money, time or date&#x20;

* a Value Object is used to represent a value or set of values and is immutable

DTO is used to transfer data between different layers or components

* DTOs are usually simple objects that contain only data, not behavior.

Dependent Object: is contained inside another object, part of "has a"

* A Dependent Object doesn't have its own identity or lifecycle but depends on another object to exist.
