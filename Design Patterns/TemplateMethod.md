# Template Method

When a class start to repeat the login like in that example

```C#
public double CalculateDiscount(Order order)
{
     if(order.Items.Count() >= 5)
        {
            return order.Value * 0.12;
        }
        return NextDiscount.Calculate(order);
}
```

Every discount is going to do the same logic, check for a contidion to apply the discount, if false them call NextDiscount.

```C#
public abstract class DiscountTemplate : IDiscount
{
    public IDiscount NextDiscount {get; init}

    public DiscountTemplate(IDiscount discount)
    {
        NextDiscount = discount;
    }

    protected abstract bool ConditionToApplyDiscount(Order order);
    protected abstract double ApplyDiscount(Order order);

    public double CalculateDiscount(Order order)
    {
        if(ConditionToApplyDiscount(order))
        {
            return ApplyDiscount(order);
        }
        return NextDiscount.CalculateDiscount(order);
    }
}
```

With that template, the concrete class should look like

```C#
public class MoreThanFiveItemsDiscount : DiscountTemplate
{
    public MoreThanFiveItemsDiscount(IDiscount discount) : base(discount) { }

    protected override bool ConditionToApplyDiscount(Order order) => order.Items.Count() >= 5;

    protected override double ApplyDiscount(Order order) => order.Value * 0.12;
}
```