# Comparable vs Comparator

## Comparable

`Comparable` interface has a single method `compareTo()` which is used to compare the value of an object with the one supplied in the method. `Comparable` interface defines the natural or default ordering of the objects of a class. On defining the `compareTo()` method in the class, we need to make sure that the return value can be:

- **Negative**: Returns negative if the object which is to be compared is less than the supplied object value.
- **Positive**: Returns positive if the object which is to be compared is greater than the supplied object value.
- **Zero**: Returns zero, if the object which is compared is equal to the supplied object value.

## Comparator

`Comparator` interface is used for the scenarios when we don’t want to use the natural ordering and want the ordering/ sorting of elements according to the specific requirements. As we cannot make any changes to the `compareTo()` method of `Comparable` interface, the `Comparator` interface provides a method `compare()` which is used to sort the objects of collection. It takes 2 arguments in the method and return value can be:

- **Negative**: Returns a negative value if the value of the first argument is less than the value of the second argument.
- **Positive**: Returns a positive value if the value of the first argument is greater than the value of the second argument.
- **Zero**: Returns a zero value if the value of the first argument is equal to the value of the second argument.

## Conclusion

| Comparable  | Comparator  |
|---|---|
| Comparable affects the original class, i.e., the actual class is modified. | Comparator doesn't affect the original class, i.e., the actual class is not modified. |
| Comparable provides compareTo() method to sort elements. | Comparator provides compare() method to sort elements. |
| We can sort the list elements of Comparable type by Collections.sort(List) method. | We can sort the list elements of Comparator type by Collections.sort(List, Comparator) method. |
| Can sort the elements of collection on the basis of the single element | Can sort on the basis of multiple elements of the collections at a time |

## Links
https://www.educba.com/comparable-vs-comparator/    
https://www.baeldung.com/java-comparator-comparable    
https://www.javatpoint.com/difference-between-comparable-and-comparator    
https://dev.to/micosmin/understanding-comparable-and-comparator-3l0i  
https://www.javaworld.com/article/3323403/java-challengers-5-sorting-with-comparable-and-comparator-in-java.html  
