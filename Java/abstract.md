## Abstract

### Methods

1. Abstract methods only provide the **declaration** or **blueprint** for **classes** or **interfaces** but contains **no implementation**.

2. Abstract methods are sometimes referred to as **child class responsibility**.

3. Abstract Static methods are not allowed as abstract methods need to be overriden but static methods cannot be overriden.

> Note: If a class contains any abstract method it should also be declared as abstract.

### Classes

1. Abstract methods only provide the **declaration** or **blueprint** for **classes** or **interfaces** but contains **no implementation**.

2. All the **child** classes **always** have to **override** the abstract methods of the abstract class upon **inheriting**.

3. Abstract classes can have **constructors** which can be called from the child class in the same old fashion way using **super()**. It can be used to initialize some final values.

4. **Abstract Constructors** are not allowed.

> Note: Abstract classes cannot be **instantiated** i.e., you cannot create objects of astract classes.

5. Although abstract classes cannot be used to create objects it can be used as reference variable type.

6. Abstract classes need to be extended so their methods can be overriden but the final keyword prevents this inheritance that's why **we cannot have final abstract classes**.

```java
// abstract class
abstract public class Parent {
    int age;
    final int VALUE;

    // constructor
    public Parent(int age, int value) {
        this.age = age;
        VALUE = value;
    }

    // static methods
    static void greet() {
        System.out.println("Hello");
    }

    // abstract methods
    abstract void career(String name);
    abstract void partner(String name, int age);
}

// child class
public class Son extends Parent {

    Son(int age, int value) {
        // calling abstract class constructor
        super(age, value);
    }

    @Override
    void career(String name) {
        System.out.println("I am going to be a " + name);
    }

    @Override
    void partner(String name, int age) {
        System.out.println("I love " + name + " and she is " + age);
    }
}

// Main class
public class Main {
    public static void main(String[] args) {
        Parent.greet();
        Son son = new Son(35, 56);
        son.career("Coder");
        son.partner("Netflix", 21);
    }
}
```
