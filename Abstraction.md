---
tags:
  - java
resources: Material dr.Mohmoud Gamal
---
# Notes
1. An abstract class cannot be `instantiated`. 
2. An abstract class serves as a superclass for other classes.

## Abstract Methods
3. An abstract method has `no body` and `must` be overridden in a subclass.

# Exam Point 
---
 
 1- Can an Abstract Class contain Both Abstract and Regular Methods ?

``` java
abstract class Animal {
    abstract void sound();

    void eat() {
        System.out.println("Eating");
    }
}
```
 
 Yes, This is completely valid

---

 2- If a Class Contain an Abstract Method , Must the Class Be Abstract ?

``` java
// Incorrect
class Animal {
    abstract void sound();
}
```

``` java
// Correct
abstract class Animal {
    abstract void sound();
}
```

---

3- Can an Abstract Class Have Constructors ?

``` Java
abstract class Employee {
    Employee() {
        System.out.println("Employee Constructor");
    }
}
```

> The constructor is executed when a subclass object is created

``` java
abstract class Employee {
    Employee() {
        System.out.println("Employee");
    }
}

class Developer extends Employee {
    Developer() {
        System.out.println("Developer");
    }
}
```

``` java
Developer d = new Developer();
```

``` shell
Employee
Developer
```

---

4- Can We Call super() from an Absract Class ?

``` java
abstract class Person {
    Person(String name) {
    }
}

abstract class Employee extends Person {
    Employee() {
        super("Ahmed");
    }
}
```

---

4- Can we Create a Reference of an Abstract Class 

``` Java
abstract class Animal {
    abstract void sound();
}

class Dog extends Animal {
    void sound() {
        System.out.println("Bark");
    }
    void fetch() {
    }
    int age = 3; 
}
```

``` java
Animal a = new Dog();
```

the only thing you can use it in with `a` object should be existing in the Animal or reference type 

``` java
a.fetch() // compile Error 
```

in this example the only thing use can use is 

``` java
a.sound() // because it exists in the Ref type `Animal`
```

---

5- Can an Abstract Method be Private 

``` java
abstract class Animal {
    private abstract void sound(); // compiler Error 
}
```

because subclass must override it
a private method cannot be accessed outside the class

---
 6- Can an Abstract Method be [[Static]]
``` java
abstract class Animal {
    static abstract void sound(); // compiler Error
}
```

Static methods cannot be overridden 
Abstract methods require overriding 

---

7- Can an Abstract Method be Final 

```java
abstract class Animal {
    final abstract void sound(); // compiler Error
}
```

final means `cannot be overridden` 

the same thing with class 
abstract class = must be inherited 
final class = cannot be inherited

---

8- Can an Abstract class Extend another abstract class
``` java
abstract class A {
    abstract void f();
}

abstract class B extends A {
// completely correct 
}
```

---

9- Can an Abstract class extend a Regular Class ?


``` java
class Person {
}

abstract class Employee extends Person {
// Yes , this is valid 
}
```



