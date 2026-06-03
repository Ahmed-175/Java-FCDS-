

# Notes 

you can implement multi-interface on the same class 

``` java
class A {}
class B {}
class C extends A, B {} // Not allowed
```

``` java
interface A {}
interface B {}

class C implements A, B {}
```

all methods in interface are pulic and abstract (by default)


``` java
interface Constants {
    int MAX = 100;
}
```

the same thing like 

``` java
class Constants {
    public static final int AGE = 10;
}
```


An interface can extend anthor interface 

``` java
interface A {
    void a();
}

interface B extends A {
    void b();
}
```

this is very important note 
``` java
interface Animal {
    static void info() {
        System.out.println("Animal interface");
    }
}
```

you can do this 
``` java
Animal.info()
```

you cannot do this 

``` java
Animal a = new Dog();
a.info();
```