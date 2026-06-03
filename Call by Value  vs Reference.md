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