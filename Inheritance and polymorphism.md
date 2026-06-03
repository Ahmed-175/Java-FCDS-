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
