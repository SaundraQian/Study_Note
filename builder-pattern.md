---
description: Algonquin College CST8288 note
---

# Builder pattern

The Builder Pattern uses multiple simple objects built step by step into a complex object. This type of design pattern is a creation pattern, which provides an optimal way to create objects.&#x20;

For example, if you go to KFC, the burger, coke, fries, wings, etc. are constant, but the combinations are frequently changed to produce the so-called "combo".

separates the construction object from the details of its representation or component objects

* often used to acomplex object e.g a Composite or object composed of mult objects&#x20;

basic approach:

* user uses Director class: order and use of build instructions
* ConcreteBuilder: creates the complex object
* Director can change build instructions: to obtain different types of objects
* can change the ConcreteBuilder: to build same type of object differently which means different "materials"

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>Week 2 PPT - P19</p></figcaption></figure>

similar to Factory pattern because Factory often used for creating simple objects but Builder pattern is more flexible which may make use of mult Factory classes



example and more information：[https://www.runoob.com/design-pattern/builder-pattern.html](https://www.runoob.com/design-pattern/builder-pattern.html)
