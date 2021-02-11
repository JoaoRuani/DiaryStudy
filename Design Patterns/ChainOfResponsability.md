# Chain of Responsability

When you have a "complex" decision logic or a lot of "ifs" and you don't want the "class user" to care about how it's the logic is implemented, thats a possible Chain of responsability case.

## Example

The example works with discounts, thinking that a discount can be applied in countless situations.

```C#
public class DiscountCalculator
{
    public double Calculate(Order order)
    {
        var discountChain = new MoreThanFiveItemsDicount(new MoreThan500DollarsDiscount(new NoDiscount()));
        return discountChain.Calculate(order);
    }
}

public abstract class Discount
{
    public Discount NextDiscount {get; init}
    public Discount(Discount next)
    {
        NextDiscount = next;
    }

    public abstract double Calculate(Order order);
}

public class MoreThanFiveItemsDiscount : Discount
{
    public MoreThanFiveItemsDiscount(Discount next) : base(next)
    {
    }
    public override double Calculate(Order order)
    {
        if(order.Items.Count() >= 5)
        {
            return order.Value * 0.12;
        }
        return NextDiscount.Calculate(order);
    }
}
```
We need a NoDiscount discount that will return 0 and don't call any discount before it, but it's also important to build the chain correct, because of that we have DiscountCalculator
