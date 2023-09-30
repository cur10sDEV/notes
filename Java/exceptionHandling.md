## Exception Handling

1. You can wrap your exception producing code in a **try-catch** block.

2. You can have **multiple catch blocks** to catch different types of exceptions.

3. The order of execution of multiple catch blocks matters so put sub-class exceptions first in order and then the super-class exceptions, otherwise they will be catched by super-class exception block.

4. You can **create your own exceptions**.

5. There can be only **one final block**.

6. The **throw** keyword is used to throw an exception, while the **throws** keyword is used to tell the compiler that this method or whatever may throw an exception(you can declare the type of exception it may throw).

```java
public class Main {
    public static void main(String[] args) {

        try {
            System.out.println(divide(5, 0));
        } // union type
        catch (MyException | ArithmeticException e) {
            System.out.println(e.getMessage());
        } catch (Exception e) {
            System.out.println("Normal Exception");
        } finally {
            System.out.println("This will always execute");
        }
    }

    private static int divide(int a, int b) throws MyException {
        if (b == 0) {
            throw new MyException("Cannot be divided by Zero");
        }
        return a / b;
    }
}


// custom Exception
public class MyException extends Exception {
    public MyException(String message) {
        super(message);
    }
}
```
