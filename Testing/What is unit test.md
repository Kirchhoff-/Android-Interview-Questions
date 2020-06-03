# Unit test
Unit testing is a software testing method by which individual units of source code, sets of one or more computer program modules together with associated control data, usage procedures, and operating procedures, are tested to determine whether they are fit for use. In object-oriented programming, a unit is often an entire interface, such as a class, but could be an individual method. By writing tests first for the smallest testable units, then the compound behaviors between those, one can build up comprehensive tests for complex applications.

The goal of unit testing is to isolate each part of the program and show that the individual parts are correct. A unit test provides a strict, written contract that the piece of code must satisfy. Unit testing allows the programmer to refactor code or upgrade system libraries at a later date, and make sure the module still works correctly (e.g., in regression testing). The procedure is to write test cases for all functions and methods so that whenever a change causes a fault, it can be quickly identified. Unit tests detect changes which may break a design contract.

Unit Testing Techniques:
- Black Box Testing - Using which the user interface, input and output are tested.
- White Box Testing - used to test each one of those functions behaviour is tested.
- Gray Box Testing - Used to execute tests, risks and assessment methods.

## What Makes a Good Unit Test?
- **Easy to write**. Developers typically write lots of unit tests to cover different cases and aspects of the application’s behavior, so it should be easy to code all of those test routines without enormous effort.
- **Readable**. The intent of a unit test should be clear. A good unit test tells a story about some behavioral aspect of our application, so it should be easy to understand which scenario is being tested and — if the test fails — easy to detect how to address the problem. With a good unit test, we can fix a bug without actually debugging the code!
- **Reliable**. Unit tests should fail only if there’s a bug in the system under test. That seems pretty obvious, but programmers often run into an issue when their tests fail even when no bugs were introduced. For example, tests may pass when running one-by-one, but fail when running the whole test suite, or pass on our development machine and fail on the continuous integration server. These situations are indicative of a design flaw. Good unit tests should be reproducible and independent from external factors such as the environment or running order.
- **Fast**. Developers write unit tests so they can repeatedly run them and check that no bugs have been introduced. If unit tests are slow, developers are more likely to skip running them on their own machines. One slow test won’t make a significant difference; add one thousand more and we’re surely stuck waiting for a while. Slow unit tests may also indicate that either the system under test, or the test itself, interacts with external systems, making it environment-dependent.

## Advantages and disadvantages of unit testing
Advantages:
- The earlier a problem is identified, the fewer compound errors occur.
- Costs of fixing a problem early can quickly outweigh the cost of fixing it later.
- Debugging processes are made easier.
- Developers can quickly make changes to the code base.
- Developers can also re-use code, migrating it to new projects.

Disadvantages:
- Tests will not uncover every bug.
- Unit tests only test sets of data and its functionality—it will not catch errors in integration.
- More lines of test code may need to be written to test one line of code—creating a potential time investment.
- Unit testing may have a steep learning curve, for example, having to learn how to use specific automated software tools.

Example:
```
interface Adder {
    int add(int a, int b);
}
```

```
class AdderImpl implements Adder {
    public int add(int a, int b) {
        return a + b;
    }
}
```

```
import static org.junit.Assert.*;
import org.junit.Test;

public class TestAdder {

    @Test
    public void testSumPositiveNumbersOneAndOne() {
        Adder adder = new AdderImpl();
        assert(adder.add(1, 1) == 2);
    }

    // can it add the positive numbers 1 and 2?
    @Test
    public void testSumPositiveNumbersOneAndTwo() {
        Adder adder = new AdderImpl();
        assert(adder.add(1, 2) == 3);
    }

    // can it add the positive numbers 2 and 2?
    @Test
    public void testSumPositiveNumbersTwoAndTwo() {
        Adder adder = new AdderImpl();
        assert(adder.add(2, 2) == 4);
    }

    // is zero neutral?
    @Test
    public void testSumZeroNeutral() {
        Adder adder = new AdderImpl();
        assert(adder.add(0, 0) == 0);
    }

    // can it add the negative numbers -1 and -2?
    @Test
    public void testSumNegativeNumbers() {
        Adder adder = new AdderImpl();
        assert(adder.add(-1, -2) == -3);
    }

    // can it add a positive and a negative?
    @Test
    public void testSumPositiveAndNegative() {
        Adder adder = new AdderImpl();
        assert(adder.add(-1, 1) == 0);
    }

    // how about larger numbers?
    @Test
    public void testSumLargeNumbers() {
        Adder adder = new AdderImpl();
        assert(adder.add(1234, 988) == 2222);
    }

}
```

## Links
https://en.wikipedia.org/wiki/Unit_testing  
https://www.toptal.com/qa/how-to-write-testable-code-and-why-it-matters  
https://searchsoftwarequality.techtarget.com/definition/unit-testing  
https://www.tutorialspoint.com/software_testing_dictionary/unit_testing.htm  

