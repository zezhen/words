---
title: unit test best practices
time: 2013.10.15 16:45:00
layout: post
---

![enter image description here](https://zezhen.files.wordpress.com/2013/10/road.jpg)

    A good dev must be a good test!

#### The Value of Unit Tests

1. UTs give you confidence that your code works as you expect it to work, help in long-term development, such as code refactor.

2. UTs provide excellent implicit documentation because they show exactly how the code is designed to be used.

3. new members always begin with unit test, so well-written ut code is very helpful to understand existing project.


#### How to Write Unit Tests

1. Structure of UT code
    1. Set up all conditions for testing.
        1. necessary data should be created in test class, so that the tests don’t have to rely on external env.
        2. mock up object or method call to get special result.
            * PowerMockito is very powerful, an example show in appendix.
    2. Call the method (or Trigger) being tested.
    3. Verify that the results are correct.
        * use assert statement to verify the result, if failed messages should be reported, the statement like “assert condition, message”.
    4. Clean up.
        * Any used resource or modified records should be clean up, so this UT method can repeat without any latent error.
2. Make each test case method test just one thing
    1. Any given behaviour should be specified in one and only one test.
    2. Otherwise if you later change that behaviour, you’ll have to change multiple tests.
3. Name testcase method to indicate what it test
    1. a name should describe the subject, condition and result(optional),
    2. for example: test_add_negative_positive(), from the name you can know this method is testing the add method with one negative and one positive input.
    3. you need a good method name rather than comment

#### What to Test

1. Inputs
    1. divide all possible input data into into different subsets, and choose several representatives from each subset, both valid and invalid input, so you can cover all of the conditions.
    2. Normal conditions
        * all the input make the code work right.
    3. Unexpected conditions
        1. The division, for example “a = b / c”, when “c=0” is a bad input;
        2. The reference, for example “obj.call()”, when obj is null is bad;
2. Boundary condition (CORRECT)
    1. Conformance : Does the value conform to an expected format?
    2. Ordering : Is the set of values ordered or unordered as appropriate?
    3. Range: Is the value within reasonable minimum and maximum values?
    4. Reference : Does the code refer anything external that isn’t under direct control of the code itself?
    5. Existence: Does the value exist? (e.g. is non-null, non-zero, present in a set, etc)
    6. Cardinality : Are there exactly enough values?
    * When testing an integer, a positive, negative and zero should be test, but if #1 have be test, there is no need to test 10 or 100, it’s enough.
    7. Time: Is everything happening in order? At right time? In time?
3. Other
    1. check Inverse relationship
    2. cross-check using other means

#### Properties of Well-Written UT

1. Through
    1. cover as many lines of code as possible
    2. cover each branch of conditional logic, including ternary operator.
    3. cover as many paths as possible, it is seems a very big deal.
2. Repeatable
    * Every one of your unit tests should be able to be run repeatedly and continue to produce the same results, regardless of the environment in which the tests are being run.
3. Independent
    * make each test case independent to all the other
 
Appendix

```java

import org.powermock.api.mockito.PowerMockito;
import org.powermock.core.classloader.annotations.PrepareForTest;

public class MockupRunner {

    @Test
    @PrepareForTest(Clz.class)
    public void testMethodMockUsingPowerMockito() throws Exception {
        Clz c = PowerMockito.mock(Clz.class);
        PowerMockito.when(c.func()).thenCallRealMethod();
        PowerMockito.when(c, "call").thenReturn(1);
        c.func(); // mock up call
        new Clz().func(); // normal call
    }
}

class Clz {

    public int func() {
    
        int rst = call();
        
        if(rst == 0) {
            System.out.println("normal call");
        } else {
            System.out.println("mock up call");
        }
        return 1;
    }

    private int call() {
    return 0;
    }
}
```
[PowerMockito](http://powermock.googlecode.com/svn/docs/powermock-1.3.7/apidocs/org/powermock/api/mockito/PowerMockito.html) uses a custom classloader and bytecode manipulation to enable mocking of static methods, constructors, final classes and methods, private methods, removal of static initializers and more.


