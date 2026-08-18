 Prototype is a creational pattern, that is used to create objects by copying existing objects.
 There are cases where prototype is required, such as when:
 1. propeties are to be copied from private fields.
 2. properties are to be copied from the super classes.

 Also useful in cases where
 1. Reading huge configurations(Parsing huge XMl or jsons) will be required to create a copy object.
 2. Loading ML models to create a copy of the existing object.

 Java Cloneable Interface:
 It's a marker interface
 There is a clone method in the Object class itself, that works only if cloneable interface is implemented, otherwise
 it throws CloneNotSupportedException.
 The clone method returns a shallow copy of the object. Shallow copy is when the object is copied by reference.
 Deep copy is when the object is copied by value. We can create deep copy by overriding the clone method and assigning new
 objects wherever required.

 ```java
   protected Object clone() throws CloneNotSupportedException {}
 ```

 Clone doesn't call the constructor, rather allocates memory and assigns the reference to the new object.
 The use of clone is discouraged.