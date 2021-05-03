## Representing business rules

Previously we've been representing static information about an entity.

### Special Case Pattern

- All of our code keeps checking over and over again if there is a user available
- We create a class to represent "Guest User"
- We end up with code that states its intent a little bit better
- wow, he really doesn't like conditionals. Would rather create a method that no-ops than writing an if statement.
- Hint: each time the program keeps switching on the same condition, there's a good chance there is a new object waiting and wanting to be discovered.

### Null Object Pattern

- Mimics the protocols of the other methods but discards the output

### Airline Frequent Flyer Programs

```ruby

class Member
  attr_accessor :miles
  attr_accessor :partner_miles
  attr_accessor :dollars
  attr_accessor :segments
end
```

- use case: adding eligibility logic to a class that already has a lot of responsibility
- rules can be first class parts of our domain model
- help prevent uncontrolled class expansion

### When objects are premature optimization: The Transactions Script

- why Avdi is not comfortable with service objects
- Java and Ruby moved single purpose functionality out of models and into "service objects"
- class is stateless with a "perform" method
- We're not using the class as a "class". We've "given up" with figuring out the domain model.
- [Patterns of Application Architecture by Martin Fowler](https://www.amazon.com/Patterns-Enterprise-Application-Architecture-Martin/dp/0321127420) has something called the Transactions Script

Before with a service object

```ruby

module Services
  class PurchaseMonkeys
    def self.perform(user:, card_info:, quantity: 1)
      if can_purchase_monkey?(user)
        gateway = PaymentGateway.new
        price   = Monkey.current_price
        total   = price * quantity
        gateway.charge!(total, card_info)
        MonkeyWarehouse.ship_monkeys(quantity, user.address)
      end
    end

    def self.can_purchase_monkey?(user)
      rules = ShippingRegulations.latest
      rules.can_ship_to?(user.address.postal_code)
    end
  end
end
```

After with a plain old procedure

```ruby
module Purchasing
  module_function

  def purchase_monkeys(user:, card_info:, quantity: 1)
    rules        = ShippingRegulations.latest
    gateway      = PaymentGateway.new
    price        = Monkey.current_price
    total        = price * quantity
    can_purchase = rules.can_purchase_monkey?(user.address.postal_code)
    if can_purchase
      gateway.charge!(total, card_info)
      MonkeyWarehouse.ship_monkeys(quantity, user.address)
    end
  end
end

```

Why?

> we're treating this module of transaction scripts as a staging area for logic that hasn't found its true home yet. In a way, we've de-factored our application a little bit, in preparation for a better, future organization of code.


