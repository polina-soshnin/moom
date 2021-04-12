## Representing Information with Objects

What are software objects? How we invent and share a view of reality with other programmers and the computer.

Objects as:

- Information
- Workflow
- Tying other objects together

Objects whose role is to represent information. The goal is to think about the trickier areas of information modeling.

Types of information objects:

- user input fields
- larger data model objects
- how to represent erroenous or invalid input in a system

Complex information modeling problems:

- representing collections of domain objects
- User object
- how to represent heirarchies of related concepts

Goal: think about information modeling in a different way.

- It is easier to think about your application's business model's in terms of database column types they bundle together.
- But, once a business model is conceptualized in terms of primitize data types such as strings, integers, floats, etc, the battle may be lost.

### Code smell: primitive obsession

- Problem: Using primitive data types to represent domain ideas.
- Solution: Introduce a ValueObject.
- A subset of the bigger OO smell: [NoStrings](http://wiki.c2.com/?NoStrings)
- [Primitive Obsession](http://wiki.c2.com/?PrimitiveObsession)
- Ruby: comprehensively object oriented language. Everything is an object, even things not commonly known as objects, like Strings and Arrays.
- Aggressively OO
- What to avoid and what is too easy to do: write procedures that act on dumb data.
- Example: trying to represent the duration of a course.
  - Where did it all go wrong? We chose to represent the domain concept of a "duration" as an integer. This falls apart when a duration is in weeks, months, days, etc.
  - Do you have any "primitive obsessed" fields in the applications you're working on? Yes. All of our values are assumed to be in U.S. dollars.

### Solution to primitive obsession: Whole value Pattern

> When parameterizing a business model, there is an overwhelming desire to express the parameters in the most fundamental units of computation.
> Doing this interferes with smooth and proper communication between parts of your program and its users.

- Transforming a primitive attribute into a domain-specific value object
- Whole Value pattern, introduced in 1994 by Ward Cunningham
- [CHECKS Pattern Language of Information Integrity](http://c2.com/ppr/checks.html)
- We've let the concept of "duration" never be properly modeled.
- We should treat "duration" and "money" as a first class domain model.
- Repl: https://replit.com/@InfiniteRabbit/Moom-Week-2#main.rb
- We've now embedded semantic meaning into our primitive values.

THIS

> This is one of the greatest insights we can gain when we go from using "primitive" objects to represent domain concepts, to using Whole Values. When we use an integer to model a duration, we start to think that a duration "is" an integer. But is it, really? No! A course duration is a course duration. It is a concept specific to our application domain, and while it may have a magnitude which can be expressed as an integer, that doesn't necessarily mean that a duration itself needs to behave in any way like an integer number. It really depends on our application semantics and requirements.


Pit of dispair:

- Be given requirements --> model application domain, define attributes using primitive types. Automatically default in terms of their corresponding database tables.

Pit of success:

- OO programming & domain drive design: invent objects which faithfully model our understanding of domain concepts.

Capacitor Session notes:

- Why excessive conditionals are a signal to talk to a product manager.
- You cannot have a coherent object model without a coherent product specification.
- "What is the minimum amount I can write to describe this feature?"

### Parallel Heirarchy code smell

- Problem: there is a lot of metadata associated with a new product and it's spread around everywhere.
- E.g. a list of validation errors that are parallel to the list of fields in an input form.
- Conversion functions should be idempotent.

> This is one of the more obnoxious antipatterns in user experience design. When a form asks me to edit my submission, I want to see my original entries echoed back to me.

- Parallel heirarchies introduce new kinds of coupling into our code.
- The red flag is that we're forcing the model object to manage information on behalf of its collaborators.
- How to eliminate: use the Exceptional Value pattern

### Exceptional Values

- In some cases we replace an exception with a nil return.
- However, we know nil is a semantically impoverished type and it forces us to push more responsibilties onto the containing model object.

Before:

```ruby
def Duration(raw_value)
  case raw_value
  when Duration
    raw_value
 when /A(d+)s+monthsz/i
    Months[$1.to_i]
  when /A(d+)s+weeksz/i
    Weeks[$1.to_i]
  when /A(d+)s+daysz/i
    Days[$1.to_i]
  else
    nil # or raising here
  end
end
```

After:

```ruby
def Duration(raw_value)
  case raw_value
  when Duration
    raw_value
  when /A(d+)s+monthsz/i
    Months[$1.to_i]
  when /A(d+)s+weeksz/i
    Weeks[$1.to_i]
  when /A(d+)s+daysz/i
    Days[$1.to_i]
  else
    ExceptionalValue.new(raw_value, reason: "Unrecognized format")
  end
end
```

Example course model:

When to use a struct and when to use a class? 🤔

```ruby
Course = Struct.new(:name, :duration) do
  def duration=(new_duration)
    self[:duration] = new_duration
  end
end

class Course
  attr_accessor :name, :duration
  def initialize(name, duration)
    self.name = name
    self.duration = duration
  end
end

c = Course.new("hello",2)
c.duration # => 2
c.duration = 3
c.duration # => 3
```

### RubyConf 2015 talk:
Overcoming Our Obsessions with Stringly-Typed Ruby

- https://youtu.be/7Obobjq8g_U
- https://blog.codinghorror.com/new-programming-jargon/
- A Riff on "strongly typed"
- We use strings all the time. Databases and Rails love strings. But it can get us into trouble.
- E.g. A tale of two codes.
- 

