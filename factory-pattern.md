---
description: Algonquin College CST8288 note
---

# Factory pattern

## Factory Design Pattern

factory design paatern separates responsibility/detail for creating an instance and from the class that uses the instance

factory pattern looking from a domain/business class

* use abstract class or interface with factory ethoid to create "product"
* use 2nd abstract class or interface to interact with product

factory pattern looking from the class to be created & used which create concrete class for both abstract classes or interfaces

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption><p>Week 4b PPT - P3</p></figcaption></figure>

<figure><img src=".gitbook/assets/image (16).png" alt=""><figcaption><p>Week 4b PPT - P4</p></figcaption></figure>

## Abstract Factory Design Pattern&#x20;

similar to (Simple) Factory DP but&#x20;

* AF can create one of different but related objects
* eg: calculator app creates one of mult. types:  tip calculator,  conversion calculator, desk style 4 functions with memory &&"paper tape"&#x20;

adds another abstract class or interface layer to further abstract from multiple factories

<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption><p>Week 4b PPT - P6</p></figcaption></figure>

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption><p>Week 4b PPT - P7</p></figcaption></figure>

already used some (but didn't know it)&#x20;

Files is part of Java NIO while InputStream is from older Java IO&#x20;

* class InputStream provides a constructor&#x20;
* many recent Java classes use Factory Pattern
* note use of static methods

```java
String out = String.format("value of pi ==% Math.PI); 
Connection connection = DriverManager.getConnection ( username, password); 
InputStream in = Files.newInputStream (Paths.get("src/database.
```

## Factory DP vs Builder DP

Both are creational DPs：encapsulates internal representation & construction steps

Factory DP&#x20;

* create an object in a single method call (often static&#x20;
* can be thought of as a "Wrapper" for a constructor

Builder DP&#x20;

* create object using mult. method calls&#x20;
* often uses method "chaining"
* permits optional features and/or defaults

