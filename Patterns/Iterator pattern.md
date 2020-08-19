# Iterator pattern

Iterator pattern is a design pattern in which an iterator is used to traverse a container and access the container's elements. The iterator pattern decouples algorithms from containers; in some cases, algorithms are necessarily container-specific and thus cannot be decoupled.

For example, the hypothetical algorithm *SearchForElement* can be implemented generally using a specified type of iterator rather than implementing it as a container-specific algorithm. This allows SearchForElement to be used on any container that supports the required type of iterator.

The Iterator design pattern solves problems like:
- The elements of an aggregate object should be accessed and traversed without exposing its representation (data structures).
- New traversal operations should be defined for an aggregate object without changing its interface.

Defining access and traversal operations in the aggregate interface is inflexible because it commits the aggregate to particular access and traversal operations and makes it impossible to add new operations later without having to change the aggregate interface.

How use Iterator pattern:
- Define a separate (iterator) object that encapsulates accessing and traversing an aggregate object.
- Clients use an iterator to access and traverse an aggregate without knowing its representation (data structures).

Different iterators can be used to access and traverse an aggregate in different ways. New access and traversal operations can be defined independently by defining new iterators.

## Example
We have interface `Iterator`:
```
public interface Iterator {
   public boolean hasNext();
   public Object next();
}
```

And interface that knows how to get iterator: 
```
public interface Container {
   public Iterator getIterator();
}
```

Then we create class - `CarBrandRepository` which is hold array of cars and have class `CarBrandIterator` which is known how iterate cars list:
```
public class CarBrandRepository implements Container {

    private String carBrands[] = {"Hyundai", "Mazda", "Aston martin", "Jaguar"};

    @Override
    public Iterator getIterator() {
        return new CarBrandIterator();
    }

    private class CarBrandIterator implements Iterator {

        private int index;

        @Override
        public boolean hasNext() {
            return index < carBrands.length;
        }

        @Override
        public Object next() {
            if (this.hasNext()) {
                return carBrands[index++];
            }
            return null;
        }
    }
}
```

Usage of `CarBrandIterator`:
```
public class CarIteratorDemo {

    public void iteratorExample() {
        CarBrandRepository namesRepository = new CarBrandRepository();

        for (Iterator iterator = namesRepository.getIterator(); iterator.hasNext();) {
            String name = (String) iterator.next();
            System.out.println("Car brand: " + name);
        }
    }
}
```

Output:
```
Car brand: Hyundai
Car brand: Mazda
Car brand: Aston martin
Car brand: Jaguar
```

## Links
https://en.wikipedia.org/wiki/Iterator_pattern  
https://www.geeksforgeeks.org/iterator-pattern/  
https://www.tutorialspoint.com/design_pattern/iterator_pattern.htm
