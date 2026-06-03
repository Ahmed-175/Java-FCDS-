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