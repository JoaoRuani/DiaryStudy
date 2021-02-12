# Strategy

*Strategy is a pattern that lets you define a family(extends or implements) of algorithms, put each of them into a separate class, and make their objects interchageable*


## Example

```C#
public class TaxCalculator
{
    public double ApplyTax(double value, ITax tax)
    {
        return value + tax.Apply(value);
    }
}
```