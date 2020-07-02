# JUnit 4 vs JUnit 5
JUnit 5 aims to adapt Java 8 style of coding and to be more robust and flexible than JUnit 4. One of the main ideas behind the new JUnit version is utilizing the features Java 8 brought to the table (mainly lambdas) to make everybody's life easier. 

## Architecture
JUnit 4 has everything bundled into single jar file.
Junit 5 is composed of 3 sub-projects i.e. JUnit Platform, JUnit Jupiter and JUnit Vintage.

- **JUnit Platform**. It defines the `TestEngine` API for developing new testing frameworks that runs on the platform.
- **JUnit Jupiter**.  It has all new junit annotations and `TestEngine` implementation to run tests written with these annotations.
- **JUnit Vintage**. To support running JUnit 3 and JUnit 4 written tests on the JUnit 5 platform.

Junit 4 requires Java 5 or higher.
Junit 5 requires Java 8 or higher.

## Annotations
A few annotation was changed:

| Feature | JUnit 4 | JUnit 5 |
|---|---|---|
| Execute before all test methods in the current class  | `@BeforeClass`  | `@BeforeAll`  |
| Execute after all test methods in the current class  | `@AfterClass`  | `@AfterAll`  |
| Execute before each test method  | `@Before`  | `@BeforeEach`  |
| Execute after each test method  | `@After` | `@AfterEach`  |
| Disable a test method / class  | `@Ignore`  | `@Disabled`  |
| Tagging and filtering  | `@Category`  | `@Tag`  |

The most important one is that we can no longer use `@Test` annotation for specifying expectations.
The *expected* parameter in JUnit 4:
```
@Test(expected = Exception.class)
public void shouldRaiseAnException() throws Exception {
    // ...
}
```

Now, we can use a method *assertThrows*:
```
public void shouldRaiseAnException() throws Exception {
    Assertions.assertThrows(Exception.class, () -> {
        //...
    });
}
```

The *timeout* attribute in JUnit 4:
```
@Test(timeout = 1)
public void shouldFailBecauseTimeout() throws InterruptedException {
    Thread.sleep(10);
}
```

Now, the *assertTimeout* method in JUnit 5:
```
@Test
public void shouldFailBecauseTimeout() throws InterruptedException {
    Assertions.assertTimeout(Duration.ofMillis(1), () -> Thread.sleep(10));
}
```

## Assertions
We can now write assertion messages in a lambda in JUnit 5, allowing the lazy evaluation to skip complex message construction until needed:
```
@Test
public void shouldFailBecauseTheNumbersAreNotEqual_lazyEvaluation() {
    Assertions.assertTrue(
      2 == 3, 
      () -> "Numbers " + 2 + " and " + 3 + " are not equal!");
}
```
We can also group assertions in JUnit 5:
```
@Test
public void shouldAssertAllTheGroup() {
    List<Integer> list = Arrays.asList(1, 2, 4);
    Assertions.assertAll("List is not incremental",
        () -> Assertions.assertEquals(list.get(0).intValue(), 1),
        () -> Assertions.assertEquals(list.get(1).intValue(), 2),
        () -> Assertions.assertEquals(list.get(2).intValue(), 3));
}
```

## New Annotations for Running Tests
The `@RunWith` was used to integrate the test context with other frameworks or to change the overall execution flow in the test cases in JUnit 4.

With JUnit 5, we can now use the `@ExtendWith` annotation to provide similar functionality.

As an example, to use the Spring features in JUnit 4:
```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(
  {"/app-config.xml", "/test-data-access-config.xml"})
public class SpringExtensionTest {
    /*...*/
}
```

Now, in JUnit 5 it is a simple extension:
```
@ExtendWith(SpringExtension.class)
@ContextConfiguration(
  { "/app-config.xml", "/test-data-access-config.xml" })
public class SpringExtensionTest {
    /*...*/
}
```

## @Nested
This annotation lets us group tests where it makes sense to do so. We might want to separate tests that deal with addition from tests that deal with division, multiplication, etc; and it provides us with an easy way to `@Disable` certain groups entirely. It also lets us try and make full English sentences as our test output, making it extremely readable.

```
@DisplayName("The calculator class: ")
class CalculatorTest {
    Calculator calc;

    @BeforeEach
    void init() {
        calc = new Calculator();
    }

    @Nested
    @DisplayName("when testing addition, ")
    class Addition {
        @Test
        @DisplayName("with positive numbers ")
        void positive() {
            assertEquals(100, calc.add(1,1), "the result should be the sum of the arguments");
        }

        @Test
        @DisplayName("with negative numbers ")
        void negative() {
            assertEquals(100, calc.add(-1,-1), "the result should be the sum of the arguments");
        }
    }

    @Nested
    @DisplayName("when testing division, ")
    class Division {
        @Test
        @DisplayName("with 0 as the divisor ")
        void throwsAtZero() {
            assertThrows(ArithmeticException.class, () -> calc.divide(2,0), "the method should throw and ArithmeticException");
        }
    }
}
```


## Links
https://howtodoinjava.com/junit5/junit-5-vs-junit-4/  
https://stackabuse.com/unit-testing-in-java-with-junit-5  
https://www.baeldung.com/junit-5-migration  
