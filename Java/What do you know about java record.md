# Record
JDK 14 introduces records, which are a new kind of type declaration. Like an `enum`, a record is a restricted form of a class. It’s ideal for "plain data carriers," classes that contain data not meant to be altered and only the most fundamental methods such as constructors and accessors.

Consider the following class definition:
```
final class Rectangle implements Shape {
    final double length;
    final double width;
    
    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }
    
    double length() { return length; }
    double width() { return width; }
}
```

It has the following characteristics:
- All of its members are declared final;
- Its only methods consist of a constructor, `Rectangle(double length, double width)` and two accessors, `length()` and `width()`

You can represent this class with a record:
```
record Rectangle(float length, float width) { }
```

A record consists of a name (in this example, it's `Rectangle`) and a list of the record's components (which in this example are `float length` and `float width`).

A record acquires these members automatically:
- A private `final` field for each of its components;
- A public read accessor method for each component with the same name and type of the component; in this example, these methods are `Rectangle::length()` and `Rectangle::width()`;
- A public constructor whose signature is derived from the record components list. The constructor initializes each private field from the corresponding argument;
- Implementations of the `equals()` and `hashCode()` methods, which specify that two records are equal if they are of the same type and their corresponding record components are equal;
- An implementation of the `toString()` method that includes the string representation of all the record's components, with their names.

## Compact Constructors
If you want your record's constructor to do more than initialize its private fields, you can define a custom constructor for the record. However, unlike a class constructor, a record constructor doesn't have a formal parameter list; this is called a *compact constructor*.

For example, the following record, `HelloWorld`, has one field, `message`. Its custom constructor calls `Objects.requireNonNull(message)`, which specifies that if the `message` field is initialized with a null value, then a `NullPointerException` is thrown. (Custom record constructors still initialize their record's private fields.)

```
record HelloWorld(String message) {
    public HelloWorld {
        java.util.Objects.requireNonNull(message);
    }
}
```

## Restrictions on Records
The following are restrictions on the use of records:
- Records cannot extend any class;
- Records cannot declare instance fields (other than the private `final` fields that correspond to the components of the record component list); any other declared fields must be `static`;
- Records cannot be abstract; they are implicitly `final`;
- The components of a record are implicitly final.

Beyond these restrictions, records behave like regular classes:
- You can declare them inside a class; nested records are implicitly static;
- You can create generic records;
- Records can implement interfaces;
- You instantiate records with the new keyword;
- You can declare in a record's body static methods, static fields, static initializers, constructors, instance methods, and nested types;
- You can annotate records and a record's individual components.

# Links
[Records](https://docs.oracle.com/en/java/javase/14/language/records.html)

# Further reading
[Java 14 Record Keyword](https://www.baeldung.com/java-record-keyword)

[Java Record](http://tutorials.jenkov.com/java/record.html)
