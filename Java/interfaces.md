## Interfaces

1. Two completely unrelated classes can also implement the same interface.

2. Just like the classes and abstract classes reference variables can also have interfaces as their types.

3. Interfaces can also **extends** other interfaces, and the class implemeting the interface which extends other interface **should implements all the methods** declared in both the interfaces.

4. Variables in interfaces are by default **static** and **fianl**.

5. Starting with new versions of Java, you can have **default implementation** of methods in interfaces.

6. Static interface methods should always have a body because they can't be inherited or overriden. They can be called using the interface name just like the classes.

7. **Anamoly**: Two interfaces implemented by a same class having same method definition with default implementation will give an error. This is the same problem why we're using interfaces to solve for.

8. You also can have **nested interfaces**. However one thing to note is that the **nested interface can be declared as public private or protected** but the **top level** interface can only declared as **public or the default one**.

```java
public interface Engine {
    // by default static and final so have to initialize them
    int PRICE = 0;

    // nested interface
    public interface Nested {
        boolean isStarted();
    }

    // default implementation - can be overriden
    default void start() {
        System.out.println("Vroom");
    };

    // static - can't be overriden
    static void greet() {
        System.out.println("I am a static method");
    }

    // need to be override
    void stop();
    void acc();
}
```

> ## Pro-tip :
>
> You can create specific classes for specific interfaces and implements their methods and stuff which then can be used later and also helps in mitigating the problem of having same method name in multiple inherited interfaces.

> ## Note:
>
> The dynamic lookup of method at runtime, it has little bit of an overhead in when compared to normal method invocation in java, that's why you should be careful not to use interfaces casually in performance critical code.

> Class to Class(abstract also) and Interface to Interface uses **extends** but Class to interface uses **implements** keyword for inheritance.

> ## In General - For interfaces, abstract classes and classes
>
> The **Access-Modifiers** for the **overriden methods** should be **same or better**, it can't be more restrictive. For exp- If it is protected in the parent it should be protected or public in the child, it can't be private.
