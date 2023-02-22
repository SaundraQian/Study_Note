---
description: Algonquin College CST8288 note
---

# "Bean" pattern

a programming convention:&#x20;

* no arg constructor&#x20;
* instance variables are private private SomeType thing;&#x20;
* convention for accessors mutators public SomeType getThing(); public void setThing(SomeType param);&#x20;
* implements Serializable

intended for reusable components (GUI & non GUI)

&#x20;if introspection & notifications is needed, then implement JavaBe a n API

## why "Bean" pattern is the not same as a Value Object

while both the Bean pattern and the Value Object pattern are used to encapsulate data,

the Bean pattern is often used for mutable objects with getter and setter methods

the Value Object pattern is used for immutable objects that encapsulate a set of values into a single object.
