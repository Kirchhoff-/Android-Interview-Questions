# Proxy pattern
The Proxy design pattern describe how to solve recurring design problems to design flexible and reusable object-oriented software, that is, objects that are easier to implement, change, test, and reuse.

A *proxy*, in its most general form, is a class functioning as an interface to something else. The proxy could interface to anything: a network connection, a large object in memory, a file, or some other resource that is expensive or impossible to duplicate. In short, a proxy is a wrapper or agent object that is being called by the client to access the real serving object behind the scenes. Use of the proxy can simply be forwarding to the real object, or can provide additional logic. In the proxy, extra functionality can be provided, for example caching when operations on the real object are resource intensive, or checking preconditions before operations on the real object are invoked. For the client, usage of a proxy object is similar to using the real object, because both implement the same interface.

The Proxy design pattern solves problems like:
- The access to an object should be controlled. For example, need to check that client have access rights before accessing the object.
- Additional functionality should be provided when accessing an object.

The Prototype design pattern describes how to solve such problems:
Define a separate Proxy object that:
- can be used as substitute for another object (`Subject`)
- implements additional functionality to control the access to this subject.

Notes:
- A proxy may hide information about the real object to the client.
- A proxy may perform optimization like on demand loading.
- A proxy may do additional house-keeping job like audit tasks.
- Proxy design pattern is also known as surrogate design pattern.

## Example
Let's say we have an interface `Record`:
```
public interface Record {
    String info();
}
```

And implementatio of `Record` interface - `DbRecord` which will be fetch data from database: 
```
import java.util.Random;

public final class DbRecord implements Record {

    private final String dbRequest;

    public DbRecord(String dbRequest) {
        this.dbRequest = dbRequest;
    }

    @Override
    public String info() {
        System.out.println("Read information from db = " + dbRequest);
        return String.valueOf(new Random().nextInt());
    }
}
```

With the help of Proxy pattern, we can create cached db record. This is help us to reduce number of request to database:
```
public final class CachedDbRecord implements Record {
    
    private final DbRecord dbRecord;

    private String info;

    public CachedDbRecord(String dbRequest) {
        this.dbRecord = new DbRecord(dbRequest);
        this.info = null;
    }

    @Override
    public String info() {
        if (info == null) {
            info = dbRecord.info();
        }

        return info;
    }
}
```

Finally example of usage:
```
public class ProxyExample {

    void proxyExample() {
        Record record = new CachedDbRecord("some db request");

        System.out.println("First request (without cache) = " + record.info());
        System.out.println("Second request (with cache) = " + record.info());
    }
}
```

Output:
```
Read information from db = some db request
First request (from database) = 2067133226 (may be different)
Second request (from cache) = 2067133226 (may be different)
```

## Conclusion
Use the Proxy pattern when:
- **When we want a simplified version of a complex or heavy object**. In this case, we may represent it with a skeleton object which loads the original object on demand, also called as lazy initialization. This is known as the Virtual Proxy
- **When the original object is present in different address space, and we want to represent it locally.**. We can create a proxy which does all the necessary boilerplate stuff like creating and maintaining the connection, encoding, decoding, etc., while the client accesses it as it was present in their local address space. This is called the Remote Proxy
- **When we want to add a layer of security to the original underlying object to provide controlled access based on access rights of the client**. This is called Protection Proxy

## Links
https://en.wikipedia.org/wiki/Proxy_pattern  
https://www.baeldung.com/java-proxy-pattern
