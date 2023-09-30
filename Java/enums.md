## Enums - Enumerations

1. ### Named group of constants you can't change.

2. By default the values inside enum are **public**, **static** and **final**.

3. You can't have **childrens** because **final** prevents inheritance.

4. Every value in enum is **zero-indexed** by default, can access by **ordinal()** method on the value.

5. The **overall type** is the name declared of the enum.

6. Can also have **constructor**. The constructor will be either **private or default**, no public or protected.

7. Constructor is **only called** whenever **first time** you access the enum, and is called **number of times** the enum contains the values i.e., **size of the enum**.

8. All the enum **expilicitly extends java.lang.enum class**, as java dosen't support **multiple inheritance** so an **enum can't extend** anything else.

9. An enum **cannot** be a **superclass**.

10. An enum can **implement multiple interfaces**.

```java
public class Basic {
    enum Week implements A {
        Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday;
        // overall type is Week

        // constructor
        Week() {
            System.out.println("Constructor is called for " + this);
        }

        @Override
        public void hello() {
            System.out.println("Hey!");
        }
    }

    public static void main(String[] args) {
        Week week = Week.Monday;
        System.out.println(week.ordinal()); // returns the index

        week.hello();

        for (Week day : Week.values()) {
            System.out.println(day);
        }
    }
}

```
