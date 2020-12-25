# EnumMap
A specialized `Map` implementation for use with enum type keys. All of the keys in an enum map must come from a single enum type that is specified, explicitly or implicitly, when the map is created. Enum maps are represented internally as arrays. This representation is extremely compact and efficient.

Enum maps are maintained in the *natural order* of their keys (the order in which the enum constants are declared). This is reflected in the iterators returned by the collections views (`keySet()`, `entrySet()`, and `values()`).

Iterators returned by the collection views are *weakly consistent*: they will never throw `ConcurrentModificationException` and they may or may not show the effects of any modifications to the map that occur while the iteration is in progress.

Null keys are not permitted. Attempts to insert a `null` key will throw `NullPointerException`. Attempts to test for the presence of a `null` key or to remove one will, however, function properly. Null values are permitted.

Like most collection implementations `EnumMap` is not synchronized. If multiple threads access an enum map concurrently, and at least one of the threads modifies the map, it should be synchronized externally.  This is typically accomplished by synchronizing on some object that naturally encapsulates the enum map. If no such object exists, the map should be "wrapped" using the `Collections.synchronizedMap(java.util.Map<K, V>)` method. This is best done at creation time, to prevent accidental unsynchronized access:

```
Map<EnumKey, V> m= Collections.synchronizedMap(new EnumMap<EnumKey, V>(...));
```

Implementation note: All basic operations execute in constant time. They are likely (though not guaranteed) to be faster than their `HashMap` counterparts.

## EnumSet vs EnumMap
Both the `EnumSet` and `EnumMap` class provides data structures to store enum values. However, there exist some major differences between them:
- `EnumSet` is represented internally as a sequence of bits, whereas the `EnumMap` is represented internally as arrays.
- `EnumSet` is created using its predefined methods like `allOf()`, `noneOf()`, `of()`, etc. However, an `EnumMap` is created using its constructor.

## Example of usage
```
import java.util.EnumMap;

public class EnumMapExample {

  enum Seasons { SUMMER, AUTUMN, WINTER, SPRING }

  public static void main(String[] args) {
    EnumMap<Seasons, Integer> map = new EnumMap(Seasons.class);

    //Using put method
    map.put(Seasons.SUMMER, 1);
    map.put(Seasons.AUTUMN, 2);
    map.put(Seasons.WINTER, 3);
    map.put(Seasons.SPRING, 4);

    System.out.println("EnumMap values = " + map);

    //Using replace method
    map.replace(Seasons.SUMMER, 10);
    map.replace(Seasons.AUTUMN, 20);
    map.replace(Seasons.WINTER, 30);
    map.replace(Seasons.SPRING, 40);

    System.out.println("EnumMap values (after replace method) = " + map);

    //Using remove method
    map.remove(Seasons.AUTUMN);
    map.remove(Seasons.SPRING);

    System.out.println("EnumMap values (after remove method) = " + map);
  }
}
```

Output:
```
EnumMap values = {SUMMER=1, AUTUMN=2, WINTER=3, SPRING=4}
EnumMap values (after replace method) = {SUMMER=10, AUTUMN=20, WINTER=30, SPRING=40}
EnumMap values (after remove method) = {SUMMER=10, WINTER=30}
```

## Conclusion
`EnumMap` are:
- Inherits `AbstractMap` class;
- Implements `Clonable` and `Serializable` interfaces;
- Not synchronized;
- Not allowed `null` keys;
- Allowed `null` elements;
- All of the elements must come from a single enumeration type;
- High performance map implementation, likely (though not guaranteed) to be faster than their `HashMap`.

# Links
https://docs.oracle.com/javase/7/docs/api/java/util/EnumMap.html  
https://www.programiz.com/java-programming/enummap

# Next questions 
[What you know about EnumSet](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20you%20know%20about%20EnumSet.md)
