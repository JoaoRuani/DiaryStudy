**Try not to introduce dependencies on infraestructure when writing unit tests.**


# Naming your tests

The name of your test should consist of three parts:

	• The name of the method being tested.
	• The scenario under which it's being tested.
	• The expected behavior when the scenario is invoked.

Like: 

    Add_SingleNumber_ReturnsSameNumber

    Deposit_NegativeValue_ShouldThrowInvalidOperationException


# Test code patterns

## Arrange, Act, Assert

**Arrange** -> build/construct the objects or values that you're going to use

**Act** -> call the system under test

**Assert** -> test if the act happens as expected


## Given, When, Then

**Given** -> Initalize the variables that you are going to use like: var expectedResponse, var testValue;

**When** -> Call the system under test/build the system under test

**Then** -> Test if the call happens as expected


# Write minimaly passing tests

The input to be used in a unit test should be the simples possible in order to verify the behavior that you are currently testing

- Tests Become more resilient to future changes in the codebase
- Closer to testing behavior over implementation


# Avoid Magic values

Naming variables in unit tests is as important, if not more important, than naming variables in production code. Unit tests should not contain magic values like:

    Action actual = () => stringCalculator.Add("1001");


# Avoid logic in tests

When writing your unit tests avoid manual string concatenation and logical conditions such as if, while, for, switch;

Less chance to introduce a bug inside your tests.
Focus on the end result, rather than implementation details

If logic in your test seems unavoidable, consider splitting the test up into two or more different tests.

Example:

BAD

```C#
[Fact] 
public void Add MultipleNumbers_ReturnsCorrectResu1ts() 
{
    var stringCalculator = new StringCalculator();
    var expected = 0;
    var testCases = new []
    {
        "0,0,0",
        "0,1,2",
        "1,2,3",
    };
    
    foreach  (var test in testCases) 
    {
        Assert.Equal(expected, stringcalculator.Add(test)); 
        expected += 3; 
    }
  
}

```

BETTER

```C#
[Theory]
[InlineData("0, 0, 1)"]; 
[InlineData("0, 1, 2)". 3]; 
[InlineData("1, 2, 3)"], 6; 
public void Add AddMultipleNumbers_ReturnSumOfNumbers() 
{
    var stringCalculator = new StringCalculator();
    var expected = 0;
    var testCases = new []
    {
        "0,0,0",
        "0,1,2",
        "1,2,3",
    };
    
    foreach  (var test in testCases) 
    {
        Assert.Equal(expected, stringcalculator.Add(test)); 
        expected += 3; 
    }
  
}

```

# Prefer helper methods to setup and teardown

If you require a similar object or state for your tests, prefer a helper method than leveraging Setup and Teardown attributes if they exists.

- Less Confusion when reading the tests since all of the code is visible from within each test.
- Less chance of setting up too much or too little for the given test.
- Less chance of sharing state between tests, which creates unwanted dependencies between them

Prefer


```C#
private StringCalculator CreateDefaultStringCalculator()
{
    return new StringCalculator();
}
```

Than

```C#
private readonly StringCalculator stringCalculator;
public StringCalculatorTests()
{
    stringCalculator = new StringCalculator();
}
```

# Avoid multiple asserts

When writing your tests, try to include one assert per test. Common approaches to using only one assert include:
- Create a separate test for each assert;
- Use parameterized tests.
	
If one assert fails, the subsequent asserts will not be evaluated.
Ensures you are not asserting multiple cases in your tests.
Gives you the entire picture as to why your tests are failing.

    Exception:
    When asserting against an object, you might assert each property to see the state of the object 


# Stub static references

One of the principles of a unit test is that it must have full control of the system under test. This can be problematic when production code includes calls to static references (for example, DateTime.Now or WheaterApi). Consider:

```C#
public int GetDiscountedPrice(int price)
{
    if(DateTime.Now.DayOfWeek == DayOfWeek.Tuesday)
    {
        return price / 2;
    }
    return price;
}
```

You can do

```C#

public interface IDateTimeProvider
{
    DayOfWeek DayOfWeek();
}
public int GetDiscountedPrice(int price, IDateTimeProvider dateTimeProvider)
{
    if(dateTimeProvider.DayOfWeek() == DayOfWeek.Tuesday)
    {
        return price/2;
    }
    return price;
}
```

Then in you test

```C#
public void GetDiscountedPrice_NotTuesday_ReturnsFullPrice()
{
    var priceCalculator = new PriceCalculator();
    var dateTimeProviderStub = new Mock<IDateTimeProvider>();
    dateTimeProviderStub.Setup(dtp => dtp.DayOfWeek()).Returns(DayOfWeek.Monday);
    
    var actual = priceCalculator.GetDiscountedPrice(2, dateTimeProviderStub);

    Assert.Equals(2, actual);
} 

public void GetDiscountedPrice_OnTuesday_ReturnsHalfPrice()
{
    var priceCalculator = new PriceCalculator();
    var dateTimeProviderStub = new Mock<IDateTimeProvider>();
    dateTimeProviderStub.Setup(dtp => dtp.DayOfWeek()).Returns(DayOfWeek.Tuesday);

    var actual = priceCalculator.GetDiscountedPrice(2, dateTimeProviderStub);

    Assert.Equals(1, actual);
}

```


