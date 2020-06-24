# JUnit
JUnit is a unit testing framework for the Java programming language. JUnit has been important in the development of test-driven development, and is one of a family of unit testing frameworks which is collectively known as xUnit that originated with SUnit.

JUnit promotes the idea of "first testing then coding", which emphasizes on setting up the test data for a piece of code that can be tested first and then implemented. This approach is like "test a little, code a little, test a little, code a little." It increases the productivity of the programmer and the stability of program code, which in turn reduces the stress on the programmer and the time spent on debugging.

Feature of JUnit:
- JUnit is an open source framework, which is used for writing and running tests.
- Provides annotations to identify test methods.
- Provides assertions for testing expected results.
- Provides test runners for running tests.
- JUnit tests allow you to write codes faster, which increases quality.
- JUnit is elegantly simple. It is less complex and takes less time.
- JUnit tests can be run automatically and they check their own results and provide immediate feedback. There's no need to manually comb through a report of test results.
- JUnit tests can be organized into test suites containing test cases and even other test suites.
- JUnit shows test progress in a bar that is green if the test is running smoothly, and it turns red when a test fails.

A JUnit test fixture is a Java object. With older versions of JUnit, fixtures had to inherit from `junit.framework.TestCase`, but the new tests using JUnit 4 should not do this. Test methods must be annotated by the `@Test` annotation. If the situation requires it it is also possible to define a method to execute before (or after) each (or all) of the test methods with the `@Before` (or `@After`) and `@BeforeClass` (or `@AfterClass`) annotations.

```
import org.junit.*;

public class FoobarTest {
    @BeforeClass
    public static void setUpClass() throws Exception {
        // Code executed before the first test method
    }

    @Before
    public void setUp() throws Exception {
        // Code executed before each test
    }
 
    @Test
    public void testOneThing() {
        // Code that tests one thing
    }

    @Test
    public void testAnotherThing() {
        // Code that tests another thing
    }

    @Test
    public void testSomethingElse() {
        // Code that tests something else
    }

    @After
    public void tearDown() throws Exception {
        // Code executed after each test 
    }
 
    @AfterClass
    public static void tearDownClass() throws Exception {
        // Code executed after the last test method 
    }
}
```

| JUnit 4  | Description  |
|---|---|
| `@Test` | Identifies a method as a test method.  |
| `@Before`  | Executed before each test. It is used to prepare the test environment (e.g., read input data, initialize the class).  |
| `@After`  | Executed after each test. It is used to cleanup the test environment (e.g., delete temporary data, restore defaults). It can also save memory by cleaning up expensive memory structures.  |
| `@BeforeClass`  | Executed once, before the start of all tests. It is used to perform time intensive activities, for example, to connect to a database. Methods marked with this annotation need to be defined as `static` to work with JUnit.  |
| `@AfterClass` | Executed once, after all tests have been finished. It is used to perform clean-up activities, for example, to disconnect from a database. Methods annotated with this annotation need to be defined as `static` to work with JUnit.  |
| `@Ignore` or `@Ignore("Why disabled")`  | Marks that the test should be disabled. This is useful when the underlying code has been changed and the test case has not yet been adapted. Or if the execution time of this test is too long to be included. It is best practice to provide the optional description, why the test is disabled.  |
| `@Test (expected = Exception.class)`  | Fails if the method does not throw the named exception.  |
| `@Test(timeout=100)`  | Fails if the method takes longer than 100 milliseconds  |

## Links
https://junit.org/junit5/  
https://www.tutorialspoint.com/junit/junit_overview.htm  
https://www.vogella.com/tutorials/JUnit/article.html  
https://en.wikipedia.org/wiki/JUnit  
https://medium.com/@lathasreeseeni/junit-2d9857773e8  
