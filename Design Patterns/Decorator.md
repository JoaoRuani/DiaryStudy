# Decorator Pattern

*Decorator is a Structural Design Pattern that lets you attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors*

Very much used when we need to apply multiple stategies, processes or increment(decorate) some behavior to an object.


## Example

```C#
    var notifierWrapper = new EmailNotifier(new SmsNotifier(new FacebookNotifier()));
    notiferWrapper.Send("Your important notification");
```

In this case, the EmailNotifier is decorated with Sms and Facebook notifications so that all are sended. The execution will work like a stack, facebook will exectue first, them sms, ...

### How to build?

```C#
public abstract class Notifier
{
    protected readonly Notifier notifier;
    public Notifier(Notifier notifier)
    [
        this.notifier = notifier;
    ]   
    public Notifier()
    {
        notifier = null;
    }
    public abstract void Send(string notification);
}

public class SmsNotifier : Notifier
{
    public SmsNotifier(Notifier notifier) : base(notifier){}
    public SmsNotifier() : base() {}
    public override void Send(string notification)
    {
        if(notifier is not null) { notifier.Send(nofication); }
    }
}
```