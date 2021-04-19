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
require 'duration'

class DurationType < ActiveModel::Type::Value
  # takes raw value (from database or user input)
  # and turns it into a Whole Value object
  def cast(value)
    value.nil? ? Blank.new : Duration(value)
  end
  # takes Whole Value object and turns it into 
  # raw value for the data base or view layer
  # e.g. this could be called before Rails saves an object
  # into the database.
  def serialize(value)
    value.to_s
  end
end

class ValueValidator < ActiveModel::EachValidator
  def validate_each(record, attribute, value)
    if value.exceptional?
      record.errors[attribute] << (options[:message] || "is invalid #{value.reason}")
    end
  end
end

class Course < ApplicationRecord
  attribute :duration, DurationType.new
  validates :duration, value: true
end

```

- There is more overhead to building with a robust domain model in mind, but that makes your app easier to maintain and think about.
- There is a balance between robustness and flexibility.

## Nothing is Something, by Sandi Metz

- https://www.youtube.com/watch?v=9mLK_8hKii8&ab_channel=Codegram
- Coding was my day job for 35 years.
- The languages you know shape how you think about problems.
- Wrote SmallTalk apps for 12 years before writing Ruby for the first time.
- My job became to think about writing code, and what makes it good.
- Discrete lessons in object oriented design.
- "The things you know seem obvious to you." <-- YES

```ruby
1.to_s => '1', call method on an object
1.send(:to_s) => '1', send a message with an argument to an object

1+1 => 2
1.send('+',1) => 2, plus is a method and it's invoked by sending a message

```

- Sandi found the "if .. else" key words unnecessary in Ruby. In a language with objects and messages, why do you need this?
- I write OO, I want to send a message to an object.
- `if` enables a procedural mindset.
- If you come from Python or JavaScript, you're going to think this way.
- What happened if you couldn't use any `if` statements.
- Sometimes nil is nothing, in that case you should throw it away (compact it).
- But if you're sending it a message, then nil is something.
- The day you have to change the value of "no animal" to something else you end up with a maintenance nightmare and a code smell called "shotgun surgery"
- I would prefer knowing an object than duplicating behavior.
- subsituting a smarter object in for nil; the Null Object Pattern.
- The "active nothing".
- Wrap the interface of another system with the "outerface" with your inner system.
- Null Object Pattern --> decreases complexity of your code.
- Try implementing without `if` statements --> you end up with inheritance.
- If you continue down the path of inheritance, you end up duplicating code.
- Don't tell me that modules are a solution to this problem. "modules are for cross cutting concerns" you'll tell me.
- Everyone has this problem: subclassed objects that are core to their domain and added tiny bits of specialization.
- Inheritance is for specialization, not for sharing code.
- Injecting an object into the existing class to play the role of the thing that is now specialization --> this is composition.
- We've isolated the thing that varies. The key here is the thing you're varying from, it might look like nothing, but it is something. It's the identity function. You have to give it a Name (e.g. Order), you have to define the Role (orderer, formatter), and then you inject the Players.

Composition + Dependency Injection (as opposed to Inheritance)

```ruby
House.new(orderer: RandomOrder.new, formatter: EchoFormatter.new).recite

```

This is what it means to do Object Oriented Design. ✨ 💖

1. Isolate the thing that varies
2. Name the concept
3. Define the role
4. Inject the Players

- Remove the conditionals by being able to send the same messages to all of the objects in your set. (this is called polymorphism)
- Polymorphism: objects of different types can be accessed through the same interface.
- Each type can provide its own, independent implementation of this interface.
- Core concept in OOP.
- Abstractions that reveal their presence by the absence of code.

## Extreme Object Oriented Programming

- https://www.youtube.com/watch?v=FDs-sSxo2iY&feature=youtu.be&ab_channel=Codegram
- Object-Oriented vs. Functional programming paradigm
- 


