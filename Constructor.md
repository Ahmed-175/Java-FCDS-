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