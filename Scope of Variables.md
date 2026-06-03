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

The only way for `x` to exist is to create a new instance of the `Main` class.

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

This variable `x` will exist only when the class is loaded into memory. It will be loaded only once, and the class and any object created from this class will use the same value.

we will study the `static` keyword in Java very well in Lecture 7
[[Static]]