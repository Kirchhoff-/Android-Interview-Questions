# Mediator pattern
Mediator pattern defines an object that encapsulates how a set of objects interact. This pattern is considered to be a behavioral pattern due to the way it can alter the program's running behavior.

In object-oriented programming, programs often consist of many classes. Business logic and computation are distributed among these classes. However, as more classes are added to a program, especially during maintenance and/or refactoring, the problem of communication between these classes may become more complex. This makes the program harder to read and maintain. Furthermore, it can become difficult to change the program, since any change may affect code in several other classes.

With the mediator pattern, communication between objects is encapsulated within a mediator object. Objects no longer communicate directly with each other, but instead communicate through the mediator. This reduces the dependencies between communicating objects, thereby reducing coupling.

**What problems can the Mediator design pattern solve?**
- Tight coupling between a set of interacting objects should be avoided;
- It should be possible to change the interaction between a set of objects independently.

Defining a set of interacting objects by accessing and updating each other directly is inflexible because it tightly couples the objects to each other and makes it impossible to change the interaction independently from (without having to change) the objects. And it stops the objects from being reusable and makes them hard to test.

*Tightly coupled objects* are hard to implement, change, test, and reuse because they refer to and know about many different objects.

**What solution does the Mediator design pattern describe?**
- Define a separate (mediator) object that encapsulates the interaction between a set of objects;
- Objects delegate their interaction to a mediator object instead of interacting with each other directly.

The objects interact with each other indirectly through a mediator object that controls and coordinates the interaction.

This makes the objects loosely coupled. They only refer to and know about their mediator object and have no explicit knowledge of each other.

## Example

Let's create two interface `Chat` (mediator) and `ChatUser`:
```
public interface Chat {
    void addUser(ChatUser user);
    void broadcastMessage(ChatUser userFrom, String message);
}
```

```
public interface ChatUser {
    String getUserName();
    void sendMessage(Chat chat, String message);
    void receiveMessage(String message);
}
```

then create it's realization - `GroupChat`(realization of mediator pattern) and `ChatUser`
```
import java.util.LinkedList;
import java.util.List;

public final class GroupChat implements Chat {

    private final List<ChatUser> userList;

    public GroupChat() {
        this.userList = new LinkedList<>();
    }

    @Override
    public void addUser(ChatUser user) {
        userList.add(user);
    }

    @Override
    public void broadcastMessage(ChatUser userFrom, String message) {
        String resultMessage = userFrom.getUserName() + ": " + message;
        if (!resultMessage.trim().isEmpty()) {
            for (ChatUser user : userList) {
                if (user != userFrom) {
                    user.receiveMessage(resultMessage);
                }
            }
        }
    }
}
```

```
public final class NormalUser implements ChatUser {

    private final String userName;

    public NormalUser(String userName) {
        this.userName = userName;
    }

    @Override
    public String getUserName() {
        return userName;
    }

    @Override
    public void sendMessage(Chat chat, String message) {
        chat.broadcastMessage(this, message);
    }

    @Override
    public void receiveMessage(String message) {
        System.out.println(getUserName() + " received message: ");
        System.out.println(message);
    }
}
```

finally example of usage:
```
public final class MediatorExample {

    public void example() {
        Chat groupChat = new GroupChat();
        ChatUser firstUser = new NormalUser("First user");
        ChatUser secondUser = new NormalUser("Second user");
        ChatUser thirdUser = new NormalUser("Third user");

        groupChat.addUser(firstUser);
        groupChat.addUser(secondUser);
        groupChat.addUser(thirdUser);

        firstUser.sendMessage(groupChat, "Hello Chat!");
        secondUser.sendMessage(groupChat, "Hi!");
        thirdUser.sendMessage(groupChat, "Hello!");
    }
}
```

Output:
```
Second user received message: 
First user: Hello Chat!
Third user received message: 
First user: Hello Chat!
First user received message: 
Second user: Hi!
Third user received message: 
Second user: Hi!
First user received message: 
Third user: Hello!
Second user received message: 
Third user: Hello!
```

## Conclusion
**Advantage**. It limits subclassing. A mediator localizes behavior that otherwise would be distributed among several objects. Changing this behaviour requires subclassing Mediator only, Colleague classes can be reused as is.

**Disadvantage**. It centralizes control. The mediator pattern trades complexity of interaction for complexity in the mediator. Because a mediator encapsulates protocols, it can become more complex than any individual colleague. This can make the mediator itself a monolith that’s hard to maintain.

# Links
https://en.wikipedia.org/wiki/Mediator_pattern  
https://www.geeksforgeeks.org/mediator-design-pattern/

# Next questions
[What is coupling?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/General/What%20is%20coupling.md)
