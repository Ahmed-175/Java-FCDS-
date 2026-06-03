
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
