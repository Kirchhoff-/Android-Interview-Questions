# Transient

Variables may be marked `transient` to indicate that they are not part of the persistent state of an object. When an object is transferred through the network, the object needs to be 'serialized'. Serialization converts the object state to serial bytes. Those bytes are sent over the network and the object is recreated from those bytes. Member variables marked by the java `transient` keyword are not transferred; they are lost intentionally.

`transient` keyword plays an important role to meet security constraints. There are various real-life examples where we don’t want to save private data in file. Another use of transient keyword is not to serialize the variable whose value can be calculated/derived using other serialized objects or system such as age of a person, current date, etc.

Since `static` fields are not part of state of the object, there is no use/impact of using `transient` keyword with `static` variables. However there is no compilation error.

`transient` and `final` : `final` variables are directly serialized by their values, so there is no use/impact of declaring `final` variable as `transient`. There is no compile-time error though.

```
import java.io.*; 
class Test implements Serializable 
{ 
    // Normal variables 
    int i = 10, j = 20; 
  
    // Transient variables 
    transient int k = 30; 
  
    // Use of transient has no impact here 
    transient static int l = 40; 
    transient final int m = 50; 
  
    public static void main(String[] args) throws Exception 
    { 
        Test input = new Test(); 
  
        // serialization 
        FileOutputStream fos = new FileOutputStream("abc.txt"); 
        ObjectOutputStream oos = new ObjectOutputStream(fos); 
        oos.writeObject(input); 
  
        // de-serialization 
        FileInputStream fis = new FileInputStream("abc.txt"); 
        ObjectInputStream ois = new ObjectInputStream(fis); 
        Test output = (Test)ois.readObject(); 
        System.out.println("i = " + output.i); 
        System.out.println("j = " + output.j); 
        System.out.println("k = " + output.k); 
        System.out.println("l = " + output.l);   
        System.out.println("m = " + output.m); 
    } 
} 
```

Output: 

```
i = 10
j = 20
k = 0
l = 40
m = 50
```

## Links
https://docs.oracle.com/javase/8/docs/api/java/beans/Transient.html  
https://en.wikibooks.org/wiki/Java_Programming/Keywords/transient  
https://www.geeksforgeeks.org/transient-keyword-java/  
https://docs.oracle.com/javase/specs/jls/se7/html/jls-8.html  
