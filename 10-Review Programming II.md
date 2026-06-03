# 1. Introduction

In this section, you are already familiar with the general approach. My role here is to extract the most important notes and highlight key tricky questions in the Java programming language.

Throughout these notes, we will study the material using real quiz examples. Each example will be analyzed carefully to understand not only the correct solution but also the special cases and edge scenarios that often cause confusion.

---
## 2. Methods

 In Java, this behavior is completely different from JavaScript. JavaScript is a dynamically typed language, meaning variables can hold values of any type and functions can return `undefined` when no value is returned.

~~~ java
public class Main {
    public static void main(String[] args) {
// This is not JavaScript.  
// In Java, a variable cannot be assigned a void value.  
// A method with a void return type does not return anything,  
// so its result cannot be stored in a variable.  
        doNothing();
    }
    public static void doNothing() {
// some code  
    }
}
~~~

However, Java is a statically (strongly) typed language. Every variable must have a defined data type, and `void` is **not** a data type that can be used for variables. It is only used to indicate that a method does not return any value.

So, assigning the result of a `void` method to a variable is not allowed in Java and will result in a compilation **Error**.

---
## 3. Scope of Variables

In Java, variable scope is different from JavaScript. JavaScript uses _execution context_ and _scope chains_, allowing functions inside functions, where each function has its own scope and can access outer variables.

In contrast, Java is based on object-oriented programming (OOP), and **methods cannot be defined inside other methods**. To achieve a similar concept of nested scope, Java uses **blocks `{}`**, where each block creates a new local scope.

~~~ Java
public class Main {
    public static void main(String[] args) {
        int x = 4;
        {
            int y = 6;
            {
                System.out.println(x + y); // both x and y are accessible 
            }
        }
        System.out.println(y); // ERROR: y is not visible in this scope
    }
}
~~~

in this section, we have a lot of fun because this is a very tricky concept that can be tactical in quizzes.  
see the following example and tell me what the output is.

~~~ Java
public class Main {  
    int x = 599;  
    public static void main(String[] args) {  
        int x = 32;  
        System.out.println(x);  
    }  
}
~~~

actually, you expect that the output here will be x = 32 because Java prefers local variables over class variables.  
but the question is: I really want to use x in the class scope.  
how can I do this without removing the local x ?

~~~ Java
public class Main {
    int x = 599;
    public static void main(String[] args) {
     int x = 32 ;
     System.out.println(this.x); // use this keyword to ref to global x
    }
}
~~~

one of the solutions is to use the `this` keyword to reference the current object, right?!

this code is a big blunder because you say that the `this` keyword is a reference to an object, and then you use it in a static method. This is incorrect because `this` is related to an object, not a class.

this solution is a very big blunder.

~~~ Java
public class Main {
    int x = 599;
    public static void main(String[] args) {
     int x = 32 ;
     System.out.println(Main.x); // use Main keyword to access x
    }
}
~~~

this is also a bad solution because the `Main` keyword refers to the class itself. you are not changing anything. you are still using the class name to access something non-static, which does not exist yet until you create an object and use it through that object.

---
### Real Solutions

 ~~~ Java 
 public class Main {
    int x = 599;
    public static void main(String[] args) {
        int x = 32;
        Main object = new Main();
        System.out.println(object.x);
    }
}
 ~~~

this solution is good you now make object of the main class take all the data sets and this solution is good. you now create an object of the main class, which gives you access to all the data fields and methods. you can do anything you want.

but this is a waste of memory because you are creating an object inside the class itself just to access its own variables?!

~~~ Java 
public class Main {
    static  int x = 599;
    public static void main(String[] args) {
        int x = 32;
        System.out.println(Main.x);
    }
}
~~~

this is the best solution until now because you just make the `x` variable static and it becomes related to the class, not to an object. you can use it in any place.

we will study the `static` keyword in Java very well in Lecture 7  #Static  

---
# 4. Call by Value  vs Reference 

~~~ Java 
public class Main {

    public static void main(String[] args) {
        String name = "Ahmed";
        changeString(name);
        System.out.println(name); // Ahmed
    }
    public  static void changeString(String name ){
        name = "Ali";
    }
}
~~~

You already understand the difference between them, but in Java it’s important to know how things actually work and when to use **pass by reference** and **pass by value**.

In Java, everything is **passed by value**.

- For primitive types, the actual value is passed to the method.
- For objects, the value that is passed is the **reference to the object**.

---
# 5. Methods Overloading 

~~~ Java 
public class Main {
    public static void main(String[] args) {
       System.out.println(sum(4 ,5));
    }
    public int sum(int x, int y) {
        return x + y;
    }
    public double sum(int x, int y) { // here just return different type
        return x + y;
    }
}
~~~

This is not method overloading. The compiler cannot decide which method you mean when calling `sum`, so this will cause an error due to duplicate method signatures (Duplicate Methods).

~~~ Java
public class Main {
    public static void main(String[] args) {
        System.out.println(sum(4, 5));
        System.out.println(sum(4, 5.0));
    }
    public static int sum(int x, int y) {
        return x + y;
    }
    public static double sum(int x, double y) {
        return x + y;
    }
}
~~~

This is real method overloading. You are using the same method name with different parameter signatures.

But what is the output?   Actually, you are right.

- The first call `sum(4, 5)` uses the method `sum(int, int)` and returns **9**
- The second call `sum(4, 5.0)` uses the method `sum(int, double)` and returns **9.0**

~~~ Java 
public class Main {
    public static void main(String[] args) {
        System.out.println(sum(4, 5));
        System.out.println(sum(4, 5.0));
    }
 //   public static int sum(int x, int y) {
 //     return x + y;
 //   }
    public static double sum(int x, double y) {
        return x + y;
    }
}
~~~

What do you think the output will be?
This is not an error. Java performs casting in this case. The first call treats `5` as `5.0`, so it becomes a `double`.  So the output will be:
- First: `9.0`
- Second: `9.0`

---
# 6. Access Modifiers 

Access modifiers are also interesting because there are many different ideas you can learn from them.  
As you know, you should start with examples first, then explain.

~~~ Java
class Main {  
// This variable can be used only inside this class and its methods  
// It cannot be accessed from any outside class  
private int x = 1;  
  
// You can use it only inside the same package  
// This is useful in multi-package projects  
protected int y = 2;  
  
// You can use this variable anywhere  
// When you create an object, you can access it from any place  
// inside or outside the package  
public int z = 3;  
  
// Default access (no modifier)  
// It is accessible only within the same package  
int w = 3;  
}
~~~

Access modifiers are one of the most important topics.  
They introduce the first concept in [[OOP]], which is **Encapsulation**.

Encapsulation means making the variable accessible only within the class by using `private`,   so you hide the variable.  Now, no one can access the variable `x`.  
But the question is: why is this useful?

There are many ways to explain this. For example:

~~~ Java
class Person {
  public int age = 16;
  public  String name = "Ahmed Farag";

}
public class Main {
    public static void main(String[] args) {
        Person ahmed = new Person();
        // user can do any thing he want
        ahmed.age = 532;
    }
}
~~~

In real-world projects, you make `age` a `private` variable  and create methods called `Getter` and `Setter`.

Inside these methods, you control the conditions and what the user can access or modify.

---

But what is the difference between `protected` and the default modifier?  Both seem to have the same property: they are not accessible outside the package.

~~~ Java
package A;  
  
public class Parent {  
protected int x = 4;  
int y = 54; // default  
}
~~~

This `Parent` class has `x` as `protected` and `y` as default.  We will explain the difference with two examples.

~~~ Java
package A;  
  
class Employee extends Parent {  
	public void test() {  
		System.out.println(x); // accessible in the same package  
		System.out.println(y); // accessible in the same package  
	}  
}
~~~


~~~ Java
package B;  
  
class Employee extends Parent {  
	public void test() {  
		System.out.println(x); // accessible throgh inheritance (protected) 
		System.out.println(y); // Error: cannot access default member           outside the package  
}  
}
~~~

---
# 7. Constructor

~~~ Java
class Test {  
// Java creates a default constructor for you  
// It does nothing  
// It is called when you create an object  
}
~~~

When you create a class without writing a constructor, Java automatically provides a default constructor.  
But when you define any constructor, the default one no longer exists.

``` Java 
class Employee {  
    private String name;  
    private int age;  
  
    public Employee(String n, int a) {  
        name = n;  
        age = a;  
    }  
}  
  
public class Main {  
    public static void main(String[] args) {  
        // Error: default constructor does not exist now  
        Employee one = new Employee();  
    }  
}
```

When you have multiple constructors, you can call one constructor from another to perform a specific task.  

As we saw, you can set access modifiers for the class members, and this means the constructor can also be `private`.  Constructors cannot be `static`.   They are special methods that are called only when you create an object.

``` Java
class Test {
    Test() {
        this(5); // will be called the contructor(int x)
        System.out.println("A");
    }
    Test(int x) {
        System.out.println("B");
    }
}
```


``` Java
class Test {
    // This constructor is public by default (package-private in reality)
    // It can be accessed only inside the SAME PACKAGE
    public Test() {
        // Constructor chaining using this()
        // this(5) means: call another constructor in the SAME CLASS
        // IMPORTANT RULE:
        // this() MUST be the FIRST statement in constructor
        this(5);

        // This line executes AFTER Test(int x, int y) finishes
        System.out.println("A");
    }
    // Accessible ONLY inside this class
    // NOT accessible:
    // - from other classes
    // - NOT even from subclasses (inheritance cannot access it directly)
    private Test(int x) {
        // Constructor chaining again inside same class
        // Calls the constructor with (int, int)
        this(2, 5);
        // This runs after Test(int x, int y) finishes
        System.out.println("B");
    }
    // Accessible:
    // - inside same package
    // - in subclasses EVEN IF they are in different package (IMPORTANT OOP RULE)
    protected Test(int x, int y) {
        // No this() or super() here, so Java implicitly adds:
        // super(); (call to Object class constructor)
        // This constructor is part of inheritance flow
        // If a subclass calls super(x, y), this constructor will execute first
        System.out.println("c");
    }

    // Accessible ONLY inside same package
    // NOT accessible outside package even with inheritance
    Test(int x, int y, int z) {

        // Again, Java implicitly adds:
        // super();

        System.out.println("d");
    }
}
```

---

``` Java
class Parent {
    Parent() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    Child() {
        // super() - Java adds it automatically for you
        System.out.println("Child");
    }
}
```

When you inherit from `Parent`, Java automatically calls `super()` inside the constructor.
If `Parent` has a default constructor, Java inserts `super()` automatically.

But if `Parent` has a non-default constructor, you must explicitly call `super(some args)` in the `Child` constructor to make it valid.

```Java
class Parent {
    Parent() {
        System.out.println("Parent");
        return; // Good in some cases like depned on parameters
    }
}
```

as we say contructor is specail method return nothing , but if you `return something` 
is absolutely wrong 

---
# 8. Static Keyword

``` Java
public class Main {
    static int x = 54;
    public static void main(String[] args) {
      this.x = 4; // ERROR: you can't use 'this' with any static member
    }
}
```

As you know, `static` makes a variable or method related to the **class area in memory**, not to a specific object.  But you can still access it from all objects created from this class.

`this` refers only to the **current object**, so it cannot be used with static members.

``` Java
public class Main {
	// It is not static now  
	// Very important rule: you cannot access this variable from any static method like main  
	// Non-static means this variable does not exist yet until you create an object
     int x = 54;
    public static void main(String[] args) {
      System.out.println(x); // Error:x use in static methon relate to class
    }
}
```

Does this make sense?  Yes, in the above code `x` is a non-static variable, so it only exists when you create an object of the class.

That means you cannot use `x` directly inside the `main` method because `main` is static and belongs to the class, not to any object


``` Java
  class Object {
    static int counter = 0;
    public Object() {
        counter++;
        System.out.println("counter increased +1");
    }
}
public class Main {
    public static void main(String[] args) {
        Object obj1 = new Object();
        Object obj2 = new Object();
        System.out.println("obj1 : " + obj1.counter);
        System.out.println("obj2 : " + obj2.counter);
        System.out.println("Object Class : " + Object.counter);
    }
}
```

what do you think the output here? remember `static` is related to the class, not any object. `counter` is one and only one variable shared across all objects and the class.

when you see this `obj1.counter`, ask yourself: what is the counter really? this is a tricky question in quizzes.  
`counter` is static, which means it has only one value, and the last updated value is `2`.

``` terminal
counter increased +1  
counter increased +1  
obj1 : 2  
obj2 : 2  
Object Class : 2
```

---
static still have some tactics here with inhairte let's deep dive in it 

``` Java
class Parent {  
	static void show() {  
		System.out.println("Parent");  
	}  
}  
  
class Child extends Parent {  
	static void show() {  
		System.out.println("Child");  
	}  
}
```

you create a `Parent` class and define a static `show` method, then create a `Child` class that extends `Parent` and defines a `show` method with the same signature.

but is this really overriding?  answer the next question: what output will be here?

``` Java
Parent p = new Child();  
p.show();
```

think very deeply before saying anything. `show` is a static method, right? and `p` is of type `Parent`. when you write `p.show()`, the output will be `"Parent"`. what??

to understand this, you need to remember: `static` is related to the class, not the object.

java asks itself: `show` is static, right? so which class type is `p` declared as? it's `Parent`. so execution happens in the `Parent` class.

even though you did `new Child()`, you created an object referenced by `p`, but when you call any static method using an object reference, java immediately uses the reference type (compile-time type), not the actual object type, and goes to that class to resolve the static method.  so the output is:

``` terminal
Parent
```

---
when you write any thing static is immediately execute first 

``` Java
class Test {  
	static {  
		System.out.println("Static block");  
	}  
  
	Test() {  
		System.out.println("Constructor");  
	}  
}
```

when you create an object from this `Test` class, the first thing that happens is that the JVM loads the class into memory first.

``` Java 
new Test();
```

now, first the `Test` class is loaded into memory, and after that the object is created. when the class loading is finished, the first thing that gets executed is the static block inside the class.

for this reason, the output will be:

``` terminal 
Static block
Constructor
```

``` Java 
Test t1 = new Test();
Test t2 = new Test();
```

``` terminal
 Static block
 Constructor
 Constructor
```

---
# 9. Inheritance and polymorphism

``` Java 
class Parent {  
	int x = 10;  
	void show() {  
	System.out.println("Parent");  
}  
}   
class Child extends Parent {  
// int x is also inherited, so Child has its own variable called x  
// the same thing applies to method show
}
```

``` Java
Child c = new Child();  
System.out.println(c.x); // 10  
c.show(); // Parent
```

Inheritance means that a class can acquire the data and behaviors of another class, but this is not as simple as it looks at this level. There are important exceptions, and concepts like hiding and overriding appear frequently in final exams.

``` Java
class Parent {  
Parent() {}  
}  
class Child extends Parent {  
Child() {} // super() implicit  
}
```

constructor is a special method, and one of its specialties is that it is not inherited by the subclass. instead, it is called immediately when the subclass calls its own constructor using `super()`.

``` Java
class SuperParent {
    public SuperParent() {
        System.out.println("SuperParent");
    }
}

class Parent extends SuperParent {
    public Parent() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    public Child() {
        System.out.println("Child");
    }
}
```

```Java 
new Child();
```

``` terminal
SuperParent
Parent
Child
```

there are also some special cases you need to deeply understand before saying anything.  
when we talk about inheritance, you should have a deep understanding of how variables, methods, constructors, and static members are inherited, and you should also understand the effect of access modifiers on inheritance.

``` Java
class Parent {  
int x = 10;  
}  

class Child extends Parent {  
int x = 20;  
}
```

``` java
Parent p = new Child();  
System.out.println(p.x); // 10
```

variables cannot be overridden. in this example, `p` is an instance of `Child`, and the reference type is `Parent`. when you call `x` from `p`, java looks at the reference type and accesses `x` from `Parent`.

``` Java
class Parent {
}

class Child extends Parent {
    int x = 20;
}
public class Main {
    public static void main(String[] args) {
     Parent c = new Child();
     System.out.println(c.x); // Error x is not in the Parent 
    }
}
```

---

the same thing applies to methods. when you write `Parent obj = new Child()`, any variable or method you use with `obj` must exist in `Parent`. even if it exists in `Child`, you cannot access it unless it is also defined in `Parent`.

``` Java 
class Parent {
}
class Child extends Parent {
   int x = 43;
   int show(){
    return x;
   }
}

public class Main {
    public static void main(String[] args) {
     Parent c = new Child();
     System.out.println(c.show()); // Error:Parent doesn't have `show` method
    }
}
```

in the methods is absolutely different let's see how  

```Java
class Parent {
    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    void show() {
        System.out.println("Child");
    }
}
```

```Java
Parent p = new Child();
p.show();
```

with methods, the behavior is different because they can be overridden. even though `p` is declared as `Parent`, if `Child` overrides the method, the overridden version will be executed. the output will be:

``` terminal
Child
```

``` Java
class Parent {
    static void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    static void show() {
        System.out.println("Child");
    }
}
```

```Java
Parent p = new Child();  
p.show();
```

``` terminal 
Parent
```

in Java, you can write `Parent p = new Child()`; this is a typical idea of polymorphism. however, you cannot do the opposite: `Child c = new Parent()`. this would lead to using methods and variables that exist in the `Child` class but do not exist in the `Parent` object you created.

``` Java
Parent p = new Child();  
// good code you can acces now very thing in `Child` calss 
Child c = (Child) p; 
```

``` Java
Parent p = new Parent();  
Child c = (Child) p; // Runtime Error
```

what about access modifiers in inheritance? you can change them by upscaling, not the opposite.
`private` → `default` → `protected` → `public`
you cannot make something more restrictive (e.g., from `public` to `private`), but the opposite is allowed.

this is only happened for method overriding , not for everything

> When hiding or overriding a method, you **cannot reduce visibility**

even in static methods, the same rule applies.

``` Java
class Parent {}
class Child extends Parent {}

class Test {
    void show(Parent p) {
        System.out.println("Parent");
    }

    void show(Child c) {
        System.out.println("Child");
    }
}

public class Main {
    public static void main(String[] args) {
        Test t = new Test();
        Parent p = new Child();
        t.show(p);
    }
}
```


