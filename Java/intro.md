## Java OOPS

1. The default value of object when not initialized is **null**.

```java
class Main {
  public static void main(String[] args) {
    // this is just declaring
    Student stu;
    System.out.println(stu); // variable not initialized error

    // initializing
    stu = new Student();
    System.out.println(stu);
  }
}

class Student {
  int rollno;
  String name;
  float marks;
}
```

2. The **new** operator dynamically allocates the memory to the object at **runtime** and returns a **reference** to it.

3. Variables and Reference variables are stored in **stack** memory and they point to the actual value (object) in the **heap** memory.

4. We **can't access** the **memory address** because it's not allowed in java.

5. **Constructor** is used to create an instance of a class.
   - It is a special type of **function/method** inside the class that has **no explicit return type** but has the implicit return type of the class itself.
   - It's name is same as of the class.
   - If no constructor in class, the **default constructor** is called.
   - If present and takes arguments, then will **bind** these **arguments** with the respective **object**.
   - **Constructor overloading** is a technique by which we can have multiple constructors and can invoke one of them at runtime based on arguments passed.
   - Inside a contructor you can also call **another constructor**.

```java
class Main {
    public static void main(String[] args) {
        Student james = new Student(18,"James",84.4f);
        james.greet(); // Hey! my name is James

        Student stu = new Student();
        stu.greet(); // Hey! my name is Student 1

        Student stu2 = new Student(james);
        stu2.greet(); // Hey! my name is James
    }
}

class Student {
    int rollno;
    String name;
    float marks;

    Student(int rollno, String name, float marks) {
        this.rollno = rollno;
        this.name = name;
        this.marks = marks;
    }

    // constructor overloading
    Student() {
        this.rollno = 1;
        this.name = "Student 1";
        this.marks = 100f;

        // or
        // can call another constructor inside a constructor
        this (1,"Student 1",100f)
    }

    // can also pass other objects as arguments
    Student(Student other) {
        this.rollno = other.rollno;
        this.name = other.name;
        this.marks = other.marks;
    }

    void greet() {
        System.out.println("Hey! my name is " + this.name);
    }
}
```

6. **final** keyword marks any type as **constant** i.e., it cannot be modified.

   - However on thing to note is that **final variables have to be initialized**.
   - final gurantees this immutability only when these instance variables are **primitive** data types.
   - When dealing with non-primitive data types i.e., objects, you can modify the values inside the object but can't **re-assign** it.

7. ### Desctructors / Cleanup Functions
   - Java provides a **finalize** method that gets called by the garbage collector before destroying the object.
   - Java does not gurantee that the finalize method will be called.
   - You cannot explicitly call the finalize method on an object.

```java
class Main {
    public static void main(String[] args) {
        final A a = new A("john");
        a.name = "jane"; // allowed

        // error - cannot assign a value to final variable a
        a = new A("riley");

        a.finalize(); // not allowed
    }
}

class A {
    final int num = 10;
    String name;

    A (String name) {
        this.name = name;
    }

    // destructor / cleanup functions
    // finalize method - deprecated - marked for removal
    @Override
    protected void finalize() throws Throwable {
        System.out.println("Object is destroyed");
    }
}
```
