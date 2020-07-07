# Mockito
Mockito is a popular mock framework which can be used in conjunction with JUnit. If you have minimal Android dependencies and need to test specific interactions between a component and its dependency within your app, use a mocking framework to stub out external dependencies in your code. That way, you can easily test that your component interacts with the dependency in an expected way. By substituting Android dependencies with mock objects, you can isolate your unit test from the rest of the Android system while verifying that the correct methods in those dependencies are called. Mockito allows you to create and configure mock objects. Using Mockito greatly simplifies the development of tests for classes with external dependencies.

If you use Mockito in tests you typically:
- Mock away external dependencies and insert the mocks into the code under test
- Execute the code under test
- Validate that the code executed correctly

## Mock objects
Mockito provides several methods to create mock objects:
- Using the static `mock()` method.
- Using the `@Mock` annotation.

If you use the `@Mock` annotation, you must trigger the initialization of the annotated fields. The `MockitoRule` does this by calling the static method `MockitoAnnotations.initMocks(this)`. Alternatively you can use `@RunWith(MockitoJUnitRunner.class)`.

```
@RunWith(JUnit4::class)
class MainViewModelTest {

    @Mock
    lateinit var userService: UserService
    
    val userRepository = mock(UserRepository::class.java)

    @Before
    fun setUp() {
        MockitoAnnotations.initMocks(this)
        
        //Here can be used both userService and userRepository
    }
  //...
}
```

## Mock types
There are two types of mock methods - `mock()` and `spy()`

`mock()` method creates mock or fake objects. The default behavior of mock methods is to do nothing. `spy()` method, just spy or stub specific methods of a real object. If don't stub some method of mock object, then the real object method is called.

```
    @Test
    public void testMockMethod(){
        List mockList = Mockito.mock(ArrayList.class);
        mockList.add("hello world");
        Mockito.verify(mockList).add("hello world");
        assertEquals(0, mockList.size());
    }
    @Test
    public void testSpyMethod(){
        List spyList = Mockito.spy(new ArrayList());
        spyList.add("hello world");
        Mockito.verify(spyList).add("hello world");
        assertEquals(1, spyList.size());
    }
```

In the above example, when we add an object to a mocked list, the object isn't actually added as the actual add method is not called. However, when we call add on a spied list, the object is actually added as the actual method is called.

## Mocking Behavior
There are a lot of method for interaction with mock objects. Some of them:
- `when()` - is used to configure simple return behavior for a mock or spy object.
- `doReturn()` - is used when we want to return a specific value when calling a method on a mock object. The mocked method is called in case of both mock and spy objects. `doReturn()` can also be used with methods that don’t return any value.
- `thenReturn()` - is used when we want to return a specific value when calling a method on a mock object. The mocked method is called in case of mock objects, and real method in case of spy objects. `thenReturn()` always expects a return type.
- `verify()` - verify calling of specific method on mock/spy object. 

```
@Test
    public void testThenReturn(){
        //Create a mock object of the class Calculator
        Calculator mockCalculator = Mockito.mock(Calculator.class);
        //Return the value of 30 when the add method is called with the arguments 10 and 20
        Mockito.when(mockCalculator.add(10, 20)).thenReturn(30);
        //Asserts that the return value of add method with arguments 10 and 20 is 30
        assertEquals(mockCalculator.add(10, 20), 30);
    }
    @Test
    public void testDoReturn(){
        //Create a spy object of the class Calculator
        Calculator mockCalculator = Mockito.spy(new Calculator());
        //Return the value of 30 when the add method is called on the spied object with the arguments 10 and 20
        Mockito.doReturn(30).when(mockCalculator).add(10, 20);
        //Asserts that the return value of add method with arguments 10 and 20 is 30
        assertEquals(mockCalculator.add(10, 20), 30);
    }
```

## Conclusion 
Advantages & Disadvantages of Mockito:
Advantages
- We can Mock any class or interface as per our need.
- It supports Test Spies, not just Mocks, thus it can be used for partial mocking as well.
Disadvantages
- Mockito cannot test static classes. So, if you’re using Mockito, it’s recommended to change static classes to Singletons.
- Mockito cannot be used to test a private method.

## Links
https://dzone.com/articles/how-to-use-mockito-in-android  
https://developer.android.com/training/testing/unit-testing/local-unit-tests  
https://medium.com/@marco_cattaneo/unit-testing-with-mockito-on-kotlin-android-project-with-architecture-components-2059eb637912  
https://www.vogella.com/tutorials/Mockito/article.html  
https://site.mockito.org/  
https://www.javadoc.io/doc/org.mockito/mockito-core/2.16.0/org/mockito/Mockito.html  
