## Generics

1. Generics can use to make your code reusable.

```java
public class CustomGenAList<T> {
    private Object[] data;
    final private int DEFAULT_SIZE = 10;
    private int size = 0; // acting also as index

    public CustomGenAList() {
        this.data = new Object[DEFAULT_SIZE];
    }

    public void add(T value) {
        if (isFull()) {
            resize();
        }
        data[size++] = value;
    }

    private void resize() {
        Object[] temp = new Object[data.length * 2];
        for (int i = 0; i < data.length; i++) {
            temp[i] = data[i];
        }
        data = temp;
    }

    private boolean isFull() {
        return size == data.length;
    }

    public T remove() {
        T removed = (T) data[--size];
        return removed;
    }

    @Override
    public String toString() {
        Object[] arr = new Object[size];
        for (int i = 0; i < size; i++) {
            arr[i] = (T) data[i];
        }
        return Arrays.toString(arr);
    }

    public static void main(String[] args) {
        CustomGenAList<Integer> list = new CustomGenAList<>();
        for (int i = 0; i < 11; i++) {
            list.add(i * 5);
        }
        System.out.println(list);
    }
}

```

2. You can use **wildcards** to set the upper bound and lower bounds.

> The **Upper bounded** wildcard restricts the unknown type to be a **specific type or a subtype** of that type and is represented using the extends keyword. In a similar way, a **lower bounded** wildcard restricts the unknown type to be a **specific type or a super type** of that type.

> Upper bound uses **extends** keyword and lower bound uses **super**.

```java
// Now will only allow Number and its **sub - types**
// like Integer, Float, Double, etc
public class CustomGenAList<T extends Number> {
  /* code here*/
}

// upper bound
public double sumOfList(CustomGenAList<? extends Number> list) {
  double s = 0.0;
  for (int i = 0; i < list.size(); i++) {
    s += list.get(i).doubleValue();
  }
  return s;
}

// lower bound
public void addNumbers(CustomGenAList<? super Integer> list) {
  for (int i = 1; i <= 10; i++) {
    list.add(i);
  }
}
```

3. You can have **generic Interfaces**.

```java
public interface GenInterface <T> {
    void display(T value);
}
```
