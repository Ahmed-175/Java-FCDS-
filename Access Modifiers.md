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

Encapsulation means making the variable accessible only within the class by using `private`, so you hide the variable.  Now, no one can access the variable `x`.  
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