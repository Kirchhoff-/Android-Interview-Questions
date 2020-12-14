# Stack
The `Stack` class represents a last-in-first-out (LIFO) stack of objects. It extends class `Vector` with five operations that allow a vector to be treated as a stack. The usual push and pop operations are provided, as well as a method to peek at the top item on the stack, a method to test for whether the stack is empty, and a method to search the stack for an item and discover how far it is from the top. When a stack is first created, it contains no items.

`Stack` is a thread-safe class and hence involves overhead when we do not need thread safety. A more complete and consistent set of LIFO stack operations is provided by the `Deque` interface and its implementations, which should be used in preference to this class. 

A stack data structure supports the following operations:
- **Push**: Adds an element to the stack. As a result, the value of the top is incremented.
- **Pop**: An element is removed from the stack. After the pop operation, the value of the top is decremented.
- **Peek**: This operation is used to look up or search for an element. The value of the top is not modified.

Apart from the methods inherited from its parent class Vector, Stack defines the following methods:
| Method  | Description | 
|---|---|
| `boolean empty()`  | Tests if this stack is empty. Returns `true` if the stack is empty, and returns `false` if the stack contains elements.  |
| `Object peek( )`  | Returns the element on the top of the stack, but does not remove it.  | 
| `Object pop( )`  | Returns the element on the top of the stack, removing it in the process. |
| `Object push(Object element)`  | Pushes the element onto the stack. Element is also returned.  |
| `int search(Object element)`  | Searches for element in the stack. If found, its offset from the top of the stack is returned. Otherwise, -1 is returned.  |

## Example

```
public class StackExample {

  public static void main(String[] args) {
    Stack<String> stack = new Stack();
    if (stack.isEmpty()) {
      System.out.println("Stack is empty");
    }

    pushAndShow(stack, "First item");
    pushAndShow(stack, "Second item");
    pushAndShow(stack, "Third item");

    popAndShow(stack);
    popAndShow(stack);

    peekAndShow(stack);
    peekAndShow(stack);

    popAndShow(stack);

    if (stack.isEmpty()) {
      System.out.println("Stack is empty");
    }
  }


  private static void pushAndShow(Stack<String> stack, String item) {
    System.out.println("Push to stack = " + item);
    stack.push(item);
    System.out.println("Stack: " + stack);
  }

  private static void popAndShow(Stack<String> stack) {
    System.out.println("Pop from stack = " + stack.pop());
    System.out.println("Stack: " + stack);
  }

  private static void peekAndShow(Stack<String> stack) {
    System.out.println("Peek from stack = " + stack.peek());
    System.out.println("Stack: " + stack);
  }
}
```

Output:
```
Stack is empty
Push to stack = First item
Stack: [First item]
Push to stack = Second item
Stack: [First item, Second item]
Push to stack = Third item
Stack: [First item, Second item, Third item]
Pop from stack = Third item
Stack: [First item, Second item]
Pop from stack = Second item
Stack: [First item]
Peek from stack = First item
Stack: [First item]
Peek from stack = First item
Stack: [First item]
Pop from stack = First item
Stack: []
Stack is empty
```

## Conclusion 
`Stack` are:
- Inherits the `Vector` class;
- Implements List interface;
- Implements `Serializable`, `Cloneable`, `Iterable<E>`, `Collection<E>`, `List<E>`, `RandomAccess` interfaces;
- Synchronized;

# Links
https://docs.oracle.com/javase/7/docs/api/java/util/Stack.html  
https://www.softwaretestinghelp.com/java-stack-tutorial/  
https://www.geeksforgeeks.org/stack-class-in-java/  
https://www.tutorialspoint.com/java/java_stack_class.htm
