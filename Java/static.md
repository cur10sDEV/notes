## Static in Java

> Static stuff is resolved during compile time

1. Access static variables inside the class using classname instead of **this**.

2. Access static varibales outside the class using classname instead of **object reference**.

3. You can declare both **variables** and **methods** as static.

4. Static varibles and methods belong to the class and not to the object.

5. Static methods can be called on class without creating an object.
   - Example: main function of any class

```java
package com.cur10sdev.staticExample;

public class Human {
    int age;
    String name;
    int salary;
    boolean married;
    static long population;

    public Human(int age, String name, int salary, boolean married) {
        this.age = age;
        this.name = name;
        this.salary = salary;
        this.married = married;
        Human.population += 1;
    }
}
```

```java
package com.cur10sdev.staticExample;

public class Main {
    public static void main(String[] args) {
        Human rohan = new Human(21,"Rohan",35000,false);
        Human trisha = new Human(24,"Trisha",39000,false);
        System.out.println(Human.population); // 2
    }
}
```

6. **main(String[] args)** function of any class runs first before anything else. So in-order to run that first without creating the object **it has to be static**.

7. Inside any static context (static method) you can't use anything that is **non-static**, but reverse is possible.

8. But there's a way to use non-static variables and methods inside static context. Here's how:
   - Create an object inside the static context and then use this object to access non-static things inside this static context.

```java
public class Main {
    public static void main(String[] args) {
        Main.fun();
    }

    static void fun() {
        // greeting(); // error

        Main obj = new Main();
        obj.greeting(); // will run
    }

    void greeting() {
        System.out.println("Hello World!");
    }
}
```

9. Likewise, you also can't use **this** keyword inside any static context.

---

### Static Block

1. Static block is used to initialise static variables.

2. It runs only once when the class first loads (when first object is created).

```java
public class StaticBlock {
    static int a = 4;
    static int b;

    // static block runs only once when class first loads
    static {
        System.out.println("I am a static block");
        b = a * 5;
    }

    public static void main(String[] args) {
        StaticBlock obj = new StaticBlock();
        // "I am a static block"
        System.out.println(StaticBlock.a + " " + StaticBlock.b);
        // 4 20

        StaticBlock.b += 3;
        System.out.println(StaticBlock.a + " " + StaticBlock.b);
        // 4 23

        StaticBlock obj2 = new StaticBlock();
        System.out.println(StaticBlock.a + " " + StaticBlock.b);
        // 4 23
    }
}
```

---

### Inner and Outer classes

1. Outer Classes i.e., which are not inside any other class can't be static for obvious reasons as the static members depend on the other class but the outer class don't have any.

2. Inner Classes can be static as they can depend on their outer classes. They can be accessed without having an object of the outer class with ".(dot)" operator.

3. Static Inner classes can be used inside their outer class to create objects with unique identity of their own.

> Note: Static inner classes means they are independent of the outer class objects but they themselves can have instances(unique identity).

```java
public class StaticInnerClass {
    static class Test {
        String name;
        public Test(String name){
            this.name = name;
        }
    }

    public static void main(String[] args) {
        Test a = new Test("Rahul");
        Test b = new Test("Rohan");

        System.out.println(a.name); // Rahul
        System.out.println(b.name); // Rohan
    }
}
```
