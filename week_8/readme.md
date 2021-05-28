## Week 8 Notifying and Listening

Important theme:

- call and return mindset to "notification" mindset

Refactor example:

- we ask an object for a particular state
- depending on the answer, we tell it to alter its state
- "first we query, then we command"
- we should keep commands and queries separate
- second guideline: "tell, don't ask"
- "feature envy": spending more time sending messages to other objects than it does using methods of the current "self" object
- 


```ruby
require "./setup"

@customer
# => #<Customer:0x0000000204d420>
@notice
# => #<QuarterlyNotice:0x0000000204d3d0>

if @customer.active?
  if @notice.quarterly?
    @customer.clear_quarterly_notices
  end
  @customer.add_notice(@notice)
end
```

end state:

```ruby

class Customer
  # ...
  def accept_notice(notice)
    return unless active?
    if notice.quarterly?
      clear_quarterly_notices
    end
    add_notice(notice)
  end
  # ...
end
```

### Domain model events

- adding listeners. it was very difficult to debug.



