## Representing User Input, Continued

> The String is the perfect vehicle for hiding information

- When you move from a primitive value --> Whole Value Object, you make something "smarter" than an integer (e.g. a duration that can be in months, days, weeks, etc.)
- You want to work at this level when attributes have unique semantics.
- As soon as some of your values are Whole Value objects, you are in a situation where some values are primitives and some are not. This can lead to lots of special casing which is not great.
- Note: `nil` is semantically meaningless. We should **not** be assigning nil a meaning.
- If `nil` means something in a context, we should leave it nil. In Object Oriented Programming, nothing is something. In our example, nil represents a blank value, an entry in a form that someone has not filled out yet.
- We want our course to have blank fields instead of nil fields.
- Whole value objects capture the semantics of their respective contexts.
- It's all about explictly modeling a "blank" value rather than implicitly modeling a value that has not yet been assigned.

Food for thought: Think about properties of blank forms you’ve seen on web pages, or on paper in the physical world. Are the fields really “nil”? Or do they have extra information associated with them, explicitly or implicitly? Now that we are representing blanks as objects in their own right, can you think of any blank-field metadata we could move from the model object or the presentation helpers, into the blank object itself?

## Whole Values in Rails

- Whole Values comes from Ward Cunningham’s CHECKS pattern language.
- Justin Weiss, guest Rails expert. Writes Practicing Rails https://www.justinweiss.com/practicing-rails/
- How to augment Rails models with the Whole Value Pattern and Exceptional Value
- How should it mesh with the existing validations that Rails has?
- The key is to tell Rails how to translate our Whole Value types into something it can save.

```ruby
class DurationType < ActiveModel::Type::Value
  # takes raw value (from database or user input)
  # and turns it into a Whole Value object
  def cast(value)
    Duration(value)
  end
  # takes Whole Value object and turns it into 
  # raw value for the data base or view layer
  # e.g. this could be called before Rails saves an object
  # into the database.
  def serialize(value)
    value.to_s
  end
end

class Course < ApplicationRecord
  attribute :duration, DurationType.new
  ...
end

```
