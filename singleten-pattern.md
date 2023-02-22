---
description: Algonquin College CST8288 note
---

# Singleten pattern

The singleton pattern is a type of object creation pattern.

The singleton pattern ensures that a class has only one instance, and instantiates itself and provides global access to the entire instance, which is called a singleton class. e.g. database or network connection, random number source, etc

There are three key points of the singleton pattern.

* There is one instance.&#x20;
* It must create this instance by itself.&#x20;
* It must provide this instance to the entire system itself.

The singleton pattern contains only one Singleton (singleton role) class, and the internal implementation of the singleton class generates only one instance, while it provides a static getInstance() factory method that allows clients to use a unique instance of it.

The Basic approach：

* private constructor：otherwise it could be called mult. times&#x20;
* static variable: to store reference to single instance&#x20;
* &#x20;Static method: which returns reference to single instance

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption><p>Week 2 PPT - P16</p></figcaption></figure>

```java
// sigleten pattern
public class SingleObject {
 
   //create a object for SingleObject 
   private static SingleObject instance = new SingleObject();
 
   //private to avoid instantiates
   private SingleObject(){}
 
   //ahieve object
   public static SingleObject getInstance(){
      return instance;
   }
 
   public void showMessage(){
      System.out.println("Hello World!");
   }

//test
public class SingletonPatternDemo {
   public static void main(String[] args) {
 
      SingleObject object = SingleObject.getInstance();
 
      object.showMessage();
   }
}
```

more information: [https://www.runoob.com/design-pattern/singleton-pattern.html](https://www.runoob.com/design-pattern/singleton-pattern.html)
