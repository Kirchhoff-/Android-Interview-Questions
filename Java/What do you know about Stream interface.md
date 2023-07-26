# `Stream` interface
```
public interface Stream<T> extends BaseStream<T, Stream<T>>
```

The Java *Stream* API provides a functional approach to processing collections of objects. The Java Stream API was added in Java 8 along with several other functional programming features.<sup>[1](https://jenkov.com/tutorials/java-functional-programming/streams.html#:~:text=The%20Java%20Stream%20API%20provides%20a%20functional%20approach%20to%20processing%20collections%20of%20objects.%20The%20Java%20Stream%20API%20was%20added%20in%20Java%208%20along%20with%20several%20other%20functional%20programming%20features.)</sup>

`Stream` provides following features:
- `Stream` does not store elements. It simply conveys elements from a source such as a data structure, an array, or an I/O channel, through a pipeline of computational operations;
- `Stream` is functional in nature. Operations performed on a stream does not modify it's source. For example, filtering a `Stream` obtained from a collection produces a new `Stream` without the filtered elements, rather than removing elements from the source collection;
- `Stream` is lazy and evaluates code only when required;
- The elements of a stream are only visited once during the life of a stream. Like an `Iterator`, a new stream must be generated to revisit the same elements of the source.<sup>[2](https://www.javatpoint.com/java-8-stream#:~:text=Stream%20provides%20following,of%20the%20source.)</sup>

The following example illustrates an aggregate operation using `Stream`:
```
int sum = widgets.stream()
                      .filter(w -> w.getColor() == RED)
                      .mapToInt(w -> w.getWeight())
                      .sum();
```

In this example, `widgets` is a `Collection<Widget>`. We create a stream of `Widget` objects via `Collection.stream()`, filter it to produce a stream containing only the red widgets, and then transform it into a stream of int values representing the weight of each red widget. Then this stream is summed to produce a total weight.<sup>[3](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#:~:text=In%20this%20example,a%20total%20weight.)</sup>

## Why we need the `Stream` interface? 
Suppose we want to iterate over a list of integers and find out sum of all the integers greater than 10. Prior to Java 8, the approach to do it would be:
```
private static int sumIterator(List<Integer> list) {
	Iterator<Integer> it = list.iterator();
	int sum = 0;
	while (it.hasNext()) {
		int num = it.next();
		if (num > 10) {
			sum += num;
		}
	}
	return sum;
}
```

There are three major problems with the above approach:
- We just want to know the sum of integers but we would also have to provide how the iteration will take place, this is also called **external iteration** because client program is handling the algorithm to iterate over the list;
- The program is sequential in nature, there is no way we can do this in parallel easily;
- There is a lot of code to do even a simple task.

To overcome all the above shortcomings, Java 8 Stream API was introduced. We can use Java Stream API to implement **internal iteration**, that is better because java framework is in control of the iteration. **Internal iteration** provides several features such as sequential and parallel execution, filtering based on the given criteria, mapping etc. Most of the Java 8 Stream API method arguments are functional interfaces, so lambda expressions work very well with them. Let’s see how can we write above logic in a single line statement using Java Streams<sup>[4](https://www.digitalocean.com/community/tutorials/java-8-stream#java-stream:~:text=Suppose%20we%20want%20to%20iterate%20over%20a%20list%20of%20integers%20and%20find%20out%20sum%20of%20all%20the%20integers%20greater%20than%2010.%20Prior%20to%20Java%208%2C%20the%20approach%20to%20do%20it%20would%20be%3A)</sup>:
```
private static int sumStream(List<Integer> list) {
	return list.stream().filter(i -> i > 10).mapToInt(i -> i).sum();
}
```

## [Terminal and Non-Terminal Operations](https://jenkov.com/tutorials/java-functional-programming/streams.html#:~:text=a%20Stream%20instance.-,Terminal%20and%20Non%2DTerminal%20Operations,-The%20Stream%20interface)

The `Stream` interface has a selection of *terminal* and *non-terminal* operations. A *non-terminal stream operation* is an operation that adds a listener to the stream without doing anything else. A *terminal stream operation* is an operation that starts the internal iteration of the elements, calls all the listeners, and returns a result.

Here is a Java Stream example which contains both a non-terminal and a terminal operation:
```
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Stream;

public class StreamExamples {

    public static void main(String[] args) {
        List<String> stringList = new ArrayList<String>();

        stringList.add("ONE");
        stringList.add("TWO");
        stringList.add("THREE");

        Stream<String> stream = stringList.stream();

        long count = stream
            .map((value) -> { return value.toLowerCase(); })
            .count();

        System.out.println("count = " + count);

    }
}
```

The call to the `map()` method of the `Stream` interface is a non-terminal operation. It merely sets a lambda expression on the stream which converts each element to lowercase.

The call to the `count()` method is a terminal operation. This call starts the iteration internally, which will result in each element being converted to lowercase and then counted.

## [Non-Terminal Operations](https://jenkov.com/tutorials/java-functional-programming/streams.html#:~:text=non%2Dterminal%20operation.-,Non%2DTerminal%20Operations,-The%20non%2Dterminal)
The non-terminal stream operations of the Java Stream API are operations that transform or filter the elements in the stream. When you add a non-terminal operation to a stream, you get a new stream back as result. The new stream represents the stream of elements resulting from the original stream with the non-terminal operation applied. Here is an example of a non-terminal operation added to a stream - which results in a new stream:
```
List<String> stringList = new ArrayList<String>();

stringList.add("ONE");
stringList.add("TWO");
stringList.add("THREE");
    
Stream<String> stream = stringList.stream();
    
Stream<String> stringStream =
    stream.map((value) -> { return value.toLowerCase(); });
```

Notice the call to `stream.map()`. This call actually returns a new `Stream` instance representing the original stream of strings with the map operation applied. 

You can only add a single operation to a given `Stream` instance. If you need to chain multiple operations after each other, you will need to apply the second operation to the `Stream` operation resulting from the first operation. Here is how that looks:
```
Stream<String> stringStream1 =
        stream.map((value) -> { return value.toLowerCase(); });

Stream<½String> stringStream2 =
        stringStream1.map((value) -> { return value.toUpperCase(); });
```

Notice how the second call to `Stream map()` is called on the `Stream` returned by the first `map()` call.

It is quite common to chain the calls to non-terminal operations on a Java `Stream`. Here is an example of chaining the non-terminal operation calls on Java streams:
```
Stream<String> stream1 = stream
  .map((value) -> { return value.toLowerCase(); })
  .map((value) -> { return value.toUpperCase(); })
  .map((value) -> { return value.substring(0,3); });
```

## [Terminal Operations](https://jenkov.com/tutorials/java-functional-programming/streams.html#:~:text=The%20terminal%20operations)
The terminal operations of the Java `Stream` interface typicall return a single value. Once the terminal operation is invoked on a `Stream`, the iteration of the `Stream` and any of the chained streams will get started. Once the iteration is done, the result of the terminal operation is returned.

A terminal operation typically does not return a new `Stream` instance. Thus, once you call a terminal operation on a stream, the chaining of `Stream` instances from non-terminal operation ends. Here is an example of calling a terminal operation on a Java `Stream`:
```
long count = stream
  .map((value) -> { return value.toLowerCase(); })
  .map((value) -> { return value.toUpperCase(); })
  .map((value) -> { return value.substring(0,3); })
  .count();
```

It is the call to `count()` at the end of the example that is the terminal operation. Since `count()` returns a `long`, the `Stream` chain of non-terminal operations (the `map()` calls) is ended.

# Links
[Java Stream API](https://jenkov.com/tutorials/java-functional-programming/streams.html)

[Java 8 Stream](https://www.javatpoint.com/java-8-stream)

[Stream](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)

[Java 8 Stream - Java Stream](https://www.digitalocean.com/community/tutorials/java-8-stream)

# Next questions
[What is the difference between a `Stream` and an `Iterator`?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Java/What%20is%20the%20difference%20between%20a%20Stream%20and%20an%20Iterator.md)

# Further Reading
[The Java 8 Stream API Tutorial](https://www.baeldung.com/java-8-streams)
