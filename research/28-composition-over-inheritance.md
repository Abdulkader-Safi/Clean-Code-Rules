# Rule 28: Prefer composition over inheritance

## What it is

Build behaviour by combining small pieces (functions, objects passed in, components wrapping components) instead of deep class hierarchies. Use inheritance only for a true "is a" relationship that the framework expects (extending `Model` in Laravel, `ControllerBase` in ASP.NET), and keep it to one level where you can.

Don't reach for a design pattern (Strategy, Factory, Visitor, Abstract Factory) until the code needs it. A function argument is often the whole Strategy pattern.

## Why it works

The Gang of Four book itself says "favor object composition over class inheritance". Deep hierarchies couple children to every detail of the parent ("fragile base class"): change the parent and something three levels down breaks. Composition keeps each piece small and replaceable.

## Check

Is there a class hierarchy more than two levels deep? A base class that exists only to share two helper methods? A pattern with one implementation?

## Bad

```ts
class Notification { send() {} }
class EmailNotification extends Notification { }
class UrgentEmailNotification extends EmailNotification { }
class UrgentEmailNotificationWithSms extends UrgentEmailNotification { }
```

## Good

```ts
type Channel = (to: Recipient, message: Message) => Promise<void>;

async function notify(to: Recipient, message: Message, channels: Channel[]) {
  await Promise.all(channels.map(send => send(to, message)));
}

await notify(user, alert, [sendEmail, sendSms]);
```

## Fix

Replace a hierarchy with a function or object that's passed in. Replace a shared base class with plain shared functions.

Sources: Gamma, Helm, Johnson, Vlissides, *Design Patterns* (1994), p. 20. Joshua Bloch, *Effective Java*, item 18 "Favor composition over inheritance".

Evidence: convention.
