# S - Single Responsiblity Principle
Class or method should have it's own responsibilty. Like `User` handle data of user not sending email that should belong to `EmailService`
# O - Open/Close Principle
Class should be open to extension but close for modification. Like you have payment processor `PaymentProcessor` this should be support for multiple payment like (Stripe, Paypal, ...) via subclasses or plugins without changing code but don't need to modify `PaymentProcessor`.
# L - Liskov Substitution Principle
Subtype must be substitution for their base types without alterning the correctness of program. Like `Bird` have fly method but subtype is `Penguin` should not break the program when using `Bird` with subtype are `Penguin` cannot fly. Instead of, rethink the design (e.g., seprate flying behavior)
# I - Interface Segregation Principle
Clients should not be forced to depend on interfaces they dont' use. Break down large, general-purpose interface to more detail, smaller interface.
Example: `Worker` have `work()`, `eat()`, `sleep()` methods. Create serprate `Workable`, `Eatable`, `Sleepable` so `Robot` class only implement `Workable` interface
# D - Dependency Inversion Principle
High-level modules should not depend on low-level modules. Both should depend on a abstractions (interface). Abstraction should not depend on detail but detail should depend on abstractions.
Example: `NotificationService` should not depend on `EmailSender`. It should depend on `MessageSender` interface, allowing flexibility to swap in `SMSSender` or other sender implementation. 
