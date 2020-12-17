# EnumSet
A specialized `Set` implementation for use with enum types. All of the elements in an enum set must come from a single enum type that is specified, explicitly or implicitly, when the set is created. Enum sets are represented internally as bit vectors. This representation is extremely compact and efficient. The space and time performance of this class should be good enough to allow its use as a high-quality, typesafe alternative to traditional int-based "bit flags." Even bulk operations (such as `containsAll` and `retainAll`) should run very quickly if their argument is also an enum set.

The iterator returned by the `iterator` method traverses the elements in their *natural order* (the order in which the enum constants are declared). The returned iterator is *weakly consistent*: it will never throw `ConcurrentModificationException` and it may or may not show the effects of any modifications to the set that occur while the iteration is in progress.

`null` elements are not permitted. Attempts to insert a `null` element will throw `NullPointerException`. Attempts to test for the presence of a `null` element or to remove one will, however, function properly.

Like most collection implementations, `EnumSet` is not synchronized. If multiple threads access an enum set concurrently, and at least one of the threads modifies the set, it should be synchronized externally. This is typically accomplished by synchronizing on some object that naturally encapsulates the enum set. If no such object exists, the set should be "wrapped" using the `Collections.synchronizedSet(java.util.Set<T>)` method. This is best done at creation time, to prevent accidental unsynchronized access:

```
Set<MyEnum> s = Collections.synchronizedSet(EnumSet.noneOf(MyEnum.class));
```

All basic operations execute in constant time. They are likely (though not guaranteed) to be much faster than their `HashSet` counterparts. Even bulk operations execute in constant time if their argument is also an enum set.

## Creating an EnumSet Object

Since `EnumSet` is an abstract class, we cannot directly create an instance of it. It has many static factory methods that allow us to create an instance. There are *two different implementations of* `EnumSet` provided by JDK:
- `RegularEnumSet`
- `JumboEnumSet` 

These are package-private and backed by a bit vector.
`RegularEnumSet` uses a single long object to store elements of the `EnumSet`. Each bit of the long element represents an Enum value. Since the size of long is 64 bits, it can store up to 64 different elements.

`JumboEnumSet` uses an array of long elements to store elements of the `EnumSet`. The only difference from `RegularEnumSet` is `JumboEnumSet` uses a long array to store the bit vector thereby allowing more than 64 values.

The factory methods create an instance based on the number of elements:
```
if (universe.length <= 64)
    return new RegularEnumSet<>(elementType, universe);
else
    return new JumboEnumSet<>(elementType, universe);
```

`EnumSet` does not provide any public constructors, instances are created using static factory methods like:
- `allOf(size)`
- `noneOf(size)`
- `range(e1, e2)`
- `of()`

## Example of usage
```
public class EnumSetTest {

    enum Seasons { SUMMER, AUTUMN, WINTER, SPRING }

    public static void main(String[] args) {
        EnumSet<Seasons> seasons1 = EnumSet.noneOf(Seasons.class);
        EnumSet<Seasons> seasons2 = EnumSet.allOf(Seasons.class);

        //Using add method
        seasons1.add(Seasons.SUMMER);
        System.out.println("EnumSet values (seasons1) = " + seasons1);
        System.out.println("EnumSet values (seasons2) = " + seasons2);

        //Using addAll method
        seasons1.addAll(seasons2);
        System.out.println("EnumSet values (seasons1) = " + seasons1);
        System.out.println("EnumSet values (seasons2) = " + seasons2);

        //Using remove and removeAll methods
        seasons2.removeAll(seasons1);
        seasons1.remove(Seasons.AUTUMN);

        System.out.println("EnumSet values (seasons1) = " + seasons1);
        System.out.println("EnumSet values (seasons2) = " + seasons2);
    }
}
```

Output:
```
EnumSet values (seasons1) = [SUMMER]
EnumSet values (seasons2) = [SUMMER, AUTUMN, WINTER, SPRING]
EnumSet values (seasons1) = [SUMMER, AUTUMN, WINTER, SPRING]
EnumSet values (seasons2) = [SUMMER, AUTUMN, WINTER, SPRING]
EnumSet values (seasons1) = [SUMMER, WINTER, SPRING]
EnumSet values (seasons2) = []
```

## Conclusion
`EnumSet` are:
- Extends `AbstractSe`t class;
- Implements `Set` interface;
- Not synchronized;
- Not allowed `null` elements;
- All of the elements must come from a single enumeration type;
- High performance set implementation, much faster than `HashSet`.

# Links
https://docs.oracle.com/javase/7/docs/api/java/util/EnumSet.html  
https://www.geeksforgeeks.org/enumset-class-java/  
https://www.baeldung.com/java-enumset
