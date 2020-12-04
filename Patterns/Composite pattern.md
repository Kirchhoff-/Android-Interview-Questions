# Composite pattern
The composite pattern describes a group of objects that are treated the same way as a single instance of the same type of object. The intent of a composite is to "compose" objects into tree structures to represent part-whole hierarchies. Implementing the composite pattern lets clients treat individual objects and compositions uniformly.

The composite pattern solves problems like:
- A part-whole hierarchy should be represented so that clients can treat part and whole objects uniformly.
- A part-whole hierarchy should be represented as tree structure.

When defining (1) `Part` objects and (2) `Whole` objects that act as containers for `Part` objects, clients must treat them separately, which complicates client code.

The compositre pattern describes how to solve such problems: 
- Define a unified `Component` interface for both part (`Leaf`) objects and whole (`Composite`) objects.
- Individual `Leaf` objects implement the `Component` interface directly, and `Composite` objects forward requests to their child components.

This enables clients to work through the `Component` interface to treat `Leaf` and `Composite` objects uniformly: `Leaf` objects perform a request directly, and `Composite` objects forward the request to their child components recursively downwards the tree structure. This makes client classes easier to implement, change, test, and reuse.

## Example

Let's create **Component** - `Employee` interface:
```
/* Component */
public interface Employee {
    void showInfo();
}
```

then create two **Leaf** - `Manager` and `Developer`:
```
/* Leaf */
public final class Manager implements Employee {

    private final long id;
    private final String name;
    private final String position;
    private final String projectName;

    public Manager(long id, String name, String position, String projectName) {
        this.id = id;
        this.name = name;
        this.position = position;
        this.projectName = projectName;
    }

    @Override
    public void showInfo() {
        System.out.println(id + ", " + name + ", " + position + ", " + projectName);
    }
}
```

```
/* Leaf */
public final class Developer implements Employee {

    private final long id;
    private final String name;
    private final String position;

    public Developer(long id, String name, String position) {
        this.id = id;
        this.name = name;
        this.position = position;
    }

    @Override
    public void showInfo() {
        System.out.println(id + ", " + name + ", " + position);
    }
}
```

Then create **Composite** - `CompositeEmployee`:
```
import java.util.ArrayList;
import java.util.List;

/* Composite */
public final class CompositeEmployee implements Employee {

    private final List<Employee> employeeList;

    public CompositeEmployee() {
        employeeList = new ArrayList<>();
    }

    public void addEmployee(Employee employee) {
        employeeList.add(employee);
    }

    public void removeEmployee(Employee employee) {
        employeeList.remove(employee);
    }

    @Override
    public void showInfo() {
        for (Employee employee: employeeList) {
            employee.showInfo();
        }
    }
}
```

and finally, example of usage:
```
public final class CompositeExample {

    public void example() {
        Developer dev1 = new Developer(1, "John", "Android developer");
        Developer dev2 = new Developer(2, "Mike", "IOS developer");
        Developer dev3 = new Developer(3, "Jimmy", "Backend");
        Manager manager1 = new Manager(4, "Tony", "Manager", "Secret project name");

        CompositeEmployee composite1 = new CompositeEmployee();
        composite1.addEmployee(dev1);
        composite1.addEmployee(dev2);
        composite1.addEmployee(dev3);
        composite1.addEmployee(manager1);
        composite1.showInfo();

        System.out.println("---Remove one developer----");

        composite1.removeEmployee(dev2);
        composite1.showInfo();
    }
}
```

Output:
```
1, John, Android developer
2, Mike, IOS developer
3, Jimmy, Backend
4, Tony, Manager, Secret project name
---Remove one developer----
1, John, Android developer
3, Jimmy, Backend
4, Tony, Manager, Secret project name
```

## Conclusion

**When to use Composite Design Pattern?**

Composite should be used when clients ignore the difference between compositions of objects and individual objects.[1] If programmers find that they are using multiple objects in the same way, and often have nearly identical code to handle each of them, then composite is a good choice; it is less complex in this situation to treat primitives and composites as homogeneous.

- Less number of objects reduces the memory usage, and it manages to keep us away from errors related to memory like `java.lang.OutOfMemoryError`.
- Although creating an object in Java is really fast, we can still reduce the execution time of our program by sharing objects.

**When not to use Composite Design Pattern?**

- Composite Design Pattern makes it harder to restrict the type of components of a composite. So it should not be used when you don’t want to represent a full or partial hierarchy of objects.
- Composite Design Pattern can make the design overly general. It makes harder to restrict the components of a composite. Sometimes you want a composite to have only certain components. With Composite, you can’t rely on the type system to enforce those constraints for you. Instead you’ll have to use run-time checks.

# Links
https://en.wikipedia.org/wiki/Composite_pattern  
https://www.geeksforgeeks.org/composite-design-pattern/
