## Lambda Functions

1. **They are just like the arrow functions of javascript**.

2. **You can assign them to a reference variable**.

```java
Arrays.sort(list, (a,b) -> -(int)(a.marks - b.marks));
```

```java
import java.util.ArrayList;

public class LambdaFunctions {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        // adding elements
        for (int i = 0; i < 5; i++) {
            list.add(i+1);
        }
        // priniting with lambda functions
        list.forEach((item) -> System.out.println(item));

        // assigning lambda functions to reference variables
        Operation sum = (a,b) -> a+b;
        Operation prod = (a,b) -> a*b;
        Operation div = (a,b) -> a-b;
        Operation diff = (a,b) -> a/b;

        LambdaFunctions myCalc = new LambdaFunctions();

        System.out.println(myCalc.operate(5,3,sum));
        System.out.println(myCalc.operate(5,3,prod));
        System.out.println(myCalc.operate(5,3,div));
        System.out.println(myCalc.operate(5,3,diff));
    }

    private int operate(int a, int b, Operation op) {
        return op.operation(a,b);
    }
}

interface Operation {
    int operation(int a, int b);
}
```
