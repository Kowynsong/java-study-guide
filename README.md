# Java-Study-Guide

**Table of Contents**

- [Stack vs Heap](#stack-vs-heap)
- [Object Orientation](#object-orientation)
- [Interface vs Implementation Inheritance](#interface-vs-implementation-inheritance)
- [Steps of Object Creation](#steps-of-object-creation)
- [Constructors](#constructors)
- [Static vs Non-Static](#static-vs-non-static)
- [Public vs Private](#public-vs-private)
- [Overloading vs Overriding](#overloading-vs-overriding)
- [Run-time Exception vs Compiler Error](#runtime-exception-vs-compiler-error)

## Stack vs Heap

In java, there are two important areas of memory, the Stack and the Heap. 

The Stack contains:

1. **Method invocations** - when a method is called, the method lands on the top of a call stack. What is actually pushed onto the stack is the 'stack frame' which holds the state of the method (such as line of code and values of all local variables). A method stays on the stack until the method is completed (reaches its closing curly brace). For example, if method foo() calls method bar(), then bar() is stacked on top of foo().

2. **Local variables** - temporary variables that are declared inside a method. These variables will exist for as long as the method is on the stack. This also includes reference variables to objects. When an object is instantiated, the reference variable exists on the stack while the object exists on the heap.

The Heap contains:

1. **Objects** - When an object is instantiated, memory is allocated in the heap to fit the object. 
2. **Instance variables** - A variable declared inside a class but not inside a method. This includes primitive instance variables and object reference variables. Depending on the primitive, java will allocate that much space on the heap to fit the primitive instance variable. However for object reference variables, memory is only allocated for the reference variable itself since you can declare a reference variable and not assign it to an object. Once the object the reference variable is referencing is actually created, then memory is allocated for that object. 

## OOP Concepts

1. **Object** - Some entity that has properties and/or tasks performed. ex: human object can hold properties like name, height, age, etc and do tasks such as walk(), run(), read(), etc. Objects are an instance of a class
2. **Class** - A blueprint that objects follow; provides the properties and tasks that an object can hold and do. Classes cannot communicate with each other; instead objects should be created through an instance of the class so that objects can communicate.
3. **Abstraction** - Showing only essential parts and hiding the implementation details in order to only provide what the object does and not how the object handles operations. 
4. **Encapsulation** - Binding variables and methods under a single entity that is hidden to other classes and only accessible within methods of its own class. 
5. **Inheritance** - Allows classes to inherit properties and methods of other classes. Parent classes can extend attributes and behavior to child classes. ex: parent class animal with properties like name, weight and methods like eat(), sleep() can be extended with a child class dog that can inherit those features and extended further to have properties like specific breed of dogs or specific behaviors like bark(). 
3 types of inheritance: single inheritance, multi-level inheritance, hierarchical inheritance
6. **Polymorphism** - Performing the same task (a method) in different ways. ex: add() vs add(int a, int b); drawPolygon() to create a square, rectangle, or triangle with different ways.

## Interface vs Implementation Inheritance

Interface Inheritance (a.k.a what)
- Allows the generalization of code in a powerful, simply manner

Implementation Inheritance (a.k.a how)

**Pros**
- Allows code-reuse: subclasses can rely on superclasses or interfaces (using **default** keyword)
- Gives another dimension of control to subclass designers: can deccide whether or not to override default implementations

**Cons**
- Makes it harder to keep track of where something was actually implemented
- Remembering arcane rules for resolving conflicts
- Breaks encapsulation

## Steps of Object Creation

1. **Declaration** - Declare a reference variable  
2. **Creation** - create an object using the new keyword and calling the object constructor. 
3. **Assignment** - assign the reference variable to the object. It is important to remember that an object without a reference variable pointing at it will be deleted by the garbage collector. 

## Constructors

Calling a constructor instantiates an object. The only way to invoke a constructor is with the keyword **new**. Every class that you create will have a constructor. If fact, if you dont explicitly make one, the compliter will write one for you. By default it will look like:
```java
public Foo(){
}
```
It is important to note that constructors do not have a return type. If there was a return type, this would be a method. 

Constructors can be used to intialize important object states. Constructors must also have the same name as the class. With multiple constuctors, it is possible to initialize different instance variables upon instantiation. For example, you can write a default constructor which does not take a parameter and a another constructor that edits the instance variable based off input. This is known as overloading a constructor.

```java
public class Dog
{
  int weight;
  
  public Dog()
  {
    weight = 15;
  }
  
  public Dog(int addWeight)
  {
    weight = addWeight;
  }
}
```
This way you can set the dog's weight if you know its weight or use the default constructor if you are unsure.

```java
Dog randomDog = new Dog(); // This dog has weight = 15
Dog corgi = new Dog(5); // This dog has weight = 5
```

It is also important to remember that the complier will **NOT** always create a no-arg constructor if you have made a constuctor with arguments. This means that if you create constructors with arguments, you must also create the no-arg constructor. Also remember that each constructor must have a different argument list. 
For example:
```java
// not ok
public Dog(int weight, boolean isHappy){}
public Dog(int weight, boolean isSad){}

// ok (since the order is different)
public Dog(int weight, boolean isHappy){}
public Dog(boolean isHappy, int weight){}
```

## Static vs Non-Static

The keyword **static** when applied to a method allows the method to be run **without having to create an instance of that class**. This means that static methods are called using the class (ex. Math.random()) while non-static methods are called using an instance reference (ex corgi.bark()). 

Also the static method is not dependent on an instance variable and therefore an instance of the class is not required. This means that if you mark a method as static, it no longer has the ability to refer to any instance variables. So the following will result in an error:

```java
public class Dog {
  privite int size;
  
  public static void main (String[] args)
  {
    System.out.println("Size of dog is " + size); // This line will result in an error becuase the complier does not know which object's instance variable you are refering to
  }
  
  public void setSize(int s) //setter method for size
  {
    size = s;
  }
  
  public int getSize() //getter method for size
  {
    return size;
  }
```
By extension, static methods cannot call other non-static methods either becuase non-static methods usually use instance variables. Even if the non-static method being called uses no instance variables, the complier will not allow you to call it. Therefore the following will also throw an error:

```java
public class Dog {
  privite int size;
  
  public static void main (String[] args)
  {
    System.out.println("Size of dog is " + getSize()); // This line will result in an error 
  }
  
  public void setSize(int s) //setter method for size
  {
    size = s;
  }
  
  public int getSize() //getter method for size
  {
    return size;
  }
```

**tldr: Statics can't see instance variable states**

## Public vs Private

## Overloading vs Overridding

**With respect to dynamic method selection:**

Dynamic method selection only happens for **overriden** methods
- When instance method of subtype overrides class of supertype
 
Dynamic method selection does not happen for **overloaded** methods
- When some other class has two methods, one for the supertype and one for the subtype

## Runtime Exceptions vs Compiler error

A **run-time exception**, as the name implies, will only occur when the code is actually running. The code will complie and can be executed but will throw an exception. Some reasons can be:

- Using variable that are actually ```null``` (potentially causing NullPointerexception)
- Illegal indexes on arrays
- accessing resources that are unavailable 
- missing classes on classpath (at run-time)

A **compiler error** means that the code will not compile, generally due to syntax errors

- missing brackets
- missing semicolons
- accessing private fields in other classes
- missing classes on the classpath (at compile time)

## Java Collections Diagram

![Markdown Here logo](https://github.com/Souloist/Java-Study-Guide/blob/master/javaCollections.png)

