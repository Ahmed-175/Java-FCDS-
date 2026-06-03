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
