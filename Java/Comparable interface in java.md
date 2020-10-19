**Java Comparable interface** is used to compare objects and sort them according to the natural order.

**Natural ordering** is referred to as its `compareTo()` function. The `String` objects are sorted lexicographically, and the `wrapper class` objects are sorted according to their built-in `compareTo()` function (like how Integers objects are sorted in ascending order)

**compareTo()**
The `compareTo()` function compares the current object with the provided object.This function is already implemented for default wrapper classes and primitive data types; but, this function also​ needs to be implemented for user-defined classes.
It returns:

1.  a `positive integer`, if the current object is greater than the provided object.
    
2.  a  `negative integer`, if the current object is less than the provided object.
    
3.  `zero`, if the current object is equal to the provided object.

***Example***
**Sorting string**

    ArrayList<String> list = new ArrayList<>();

	list.add("E");

	list.add("A");

	list.add("C");

	list.add("B");

	list.add("D");

	Collections.sort(list);

	System.out.println(list); // [A, B, C, D, E]

**Sorting user defined objects**

    
	class Employee implements Comparable<Employee> {
		private String name;
		private int age;

		Employee(String name, int age) {
			this.name = name;
			this.age = age;
		}

		@Override
		public int compareTo(Employee employee) {
			if (this.age == employee.age)
				return 0;
			else if (this.age > employee.age)
				return 1;
			else
				return -1;
		}
		
		@Override
	    public String toString() {
	        return "[name=" + this.name + ", age=" + this.age +"]";
	    }
	}

==========================================================
                                                               
	
	List<Employee> employeeList = new ArrayList<Employee>();
    employeeList.add(new Employee("Emp1", 25));
	employeeList.add(new Employee("Emp2", 10));
	System.out.println(employeeList);	// [[name=Emp1, age=25], [name=Emp2, age=10]]
	Collections.sort(employeeList);	
	System.out.println(employeeList); // [[name=Emp2, age=10], [name=Emp1, age=25]]
