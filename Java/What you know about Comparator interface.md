# Comparator interface
`Comparator` interface is used to order(sort) the objects in the collection in your own way. It gives you the ability to decide how elements will be sorted and stored within collection and map. Comparators can be passed to a sort method (such as `Collections.sort` or `Arrays.sort`) to allow precise control over the sort order. Comparators can also be used to control the order of certain data structures (such as sorted sets or sorted maps), or to provide an ordering for collections of objects that don't have a natural ordering.

Rules for using Comparator interface:
- If you want to sort the elements of a collection, you need to implement `Comparator` interface.
- If you do not specify the type of the object in your `Comparator` interface, then, by default, it assumes that you are going to sort the objects of type `Object`. Thus, when you override the `compare()` method ,you will need to specify the type of the parameter as `Object` only.
- If you want to sort the user-defined type elements, then while implementing the `Comparator` interface, you need to specify the user-defined type generically. If you do not specify the user-defined type while implementing the interface, then by default, it assumes `Object` type and you will not be able to compare the user-defined type elements in the collection

## Example

Let's create `Student` class:
```
public final class Student {

    private final int id;
    private final String name;

    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    int getId() {
        return id;
    }

    String getName() {
        return name;
    }

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```

Then create `StudentIdComparator` class:
```
import java.util.Comparator;

public final class StudentIdComparator implements Comparator<Student> {

    @Override
    public int compare(Student student1, Student student2) {
        if (student1.getId() == student2.getId()) return 0;
        else if (student1.getId() > student2.getId()) return 1;
        else return -1;
    }
}
```

Example of usage `StudenIdComparator`

```
public class Test {

   public static void main(String[] args) {
        List<Student> studentList = new ArrayList<>();
        studentList.add(new Student(3, "Donald"));
        studentList.add(new Student(2, "Joseph"));
        studentList.add(new Student(1, "Mike"));
        Collections.sort(studentList, new StudentIdComparator());
        System.out.println("Sorted ArrayList:");
        System.out.println(studentList);

        Set<Student> studentSet = new TreeSet<>(new StudentIdComparator());
        studentSet.add(new Student(6, "Daniel"));
        studentSet.add(new Student(5, "Sam"));
        studentSet.add(new Student(4, "Kyrie"));
        System.out.println("Sorted TreeSet:");
        System.out.println(studentSet);
   }
}
```        

Output:
```
Sorted ArrayList:
[Student{id=1, name='Mike'}, Student{id=2, name='Joseph'}, Student{id=3, name='Donald'}]
Sorted TreeSet:
[Student{id=4, name='Kyrie'}, Student{id=5, name='Sam'}, Student{id=6, name='Daniel'}]
```

**Note**:
- When we are sorting elements in a collection using `Comparator` interface, we need to pass the class object that implements `Comparator` interface.
- To sort a `TreeSet` collection, this object needs to be passed in the constructor of `TreeSet`.
- If any other collection, like `ArrayList`, was used, then we need to call sort method of Collections class and pass the name of the collection and this object as a parameter.

## Links
https://www.studytonight.com/java/comparators-interface-in-java.php  
https://www.edureka.co/blog/comparable-in-java/  
https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html  
https://www.javatpoint.com/Comparator-interface-in-collection-framework  
