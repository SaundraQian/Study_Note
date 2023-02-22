---
description: Algonquin College CST8288 note
---

# Delegation pattern

## Inheritance Vs. omposition

inheritance reuse through subclassing&#x20;

* "is a" relationship&#x20;
* subclass is defined in terms of its superclass
* subclass can often see superclass internals • determined at compile time
* easy to override methods&#x20;
* can't change at runtime&#x20;
* implementation changes in superclass impact subclass cascading changes&#x20;
* alternative approach:&#x20;
* use abstract class or interface

composition reuse through association&#x20;

* "has a" relationship
* a class associates with others to gain functionality
* determined/changed at run time
* respects class interface&#x20;
* interfaces need careful design esp. to be used with many other classes&#x20;
* encapsulation respected&#x20;
* sometimes more classes needed for solution more complex

<figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption><p>Week 2 PPT - P3</p></figcaption></figure>

## Delegation pattern

inheritance:  methods in subclass can defer to methods in its superclass super.toString()&#x20;

"delegation":  technique used with composition to reuse an implementation&#x20;

* suppose object1 has a reference to another object: object2&#x20;
* &#x20;object1 uses its reference to access method(s) in object2 of course, only those methods exposed by object2&#x20;
* object1 delegates its operations to object2
  * object1 is the delegator&#x20;
  * object 2 is the delegate of object1&#x20;
* delegation is dynamic can be changed at run time&#x20;
* e.g. Window can be a shape other than Rectangle, but it can add complexity so use established patterns

```java
// object 2 (delegate)
public class RealPrinter {
	void print() {
		System.out.print("something");
	}
}

// object 1 (delegator)
public class Printer {// 委托者

	RealPrinter p = new RealPrinter(); // create delegate

	void print() {
		p.print(); // delegation
	}
}

//test
public class MainTest {

	public static void main(String[] args) {
		Printer printer = new Printer();
        printer.print();//something
	}
}
```

* delegator can delegate one or more delegate objects&#x20;
* a delegate may be delegated to mult. delegator objects&#x20;
* • delegate may expose mult. methods for invocation by delegator object(s)

more information：[https://www.yimipuzi.com/1006.html](https://www.yimipuzi.com/1006.html)
