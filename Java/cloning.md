## Cloning

### Shallow Copy

---

**Primitives will be copied as it is, but in case the object contains non-primitives than the reference variables in the copy object will just point to these non-primitives instead of making a copy**.

**This is because the change in the non-primitives of the copy object will result in the change in original object's non-primitives**.

### Deep Copy

---

**All the data is re-created as it is whether it is primitive or non-primitive**

### Implementation

---

```java
public class Human implements Cloneable{
    String name;
    int age;
    int[] arr;

    public Human(String name, int age) {
        this.age = age;
        this.name = name;
        this.arr = new int[]{1, 2, 3, 4, 5};
    }

    public Object shallowClone() throws CloneNotSupportedException {
        return super.clone();
    }

    public Object deepClone() throws CloneNotSupportedException {
        // first make a shallow copy
        Human twin = (Human) super.clone();
         // make a new arr
        twin.arr = new int[twin.arr.length];
        for (int i = 0; i < twin.arr.length; i++) {
          // copy all elements
            twin.arr[i] = this.arr[i];
        }
        return twin;
    }
}


public class Main {
    public static void main(String[] args) throws CloneNotSupportedException {
        Human me = new Human("jonas", 28);
        Human twin = (Human) me.deepClone();
        twin.arr[0] = 100;
        // no changes in this case to original object
        System.out.println(Arrays.toString(me.arr));
    }
}

```
