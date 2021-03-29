Week 1

March 29 2021

### What does it mean to be object-oriented?

Goal: think in objects. Spend time noticing the little stories in the interactions that our code implements. The stories of the workflows that our users go through. When we see a story, no matter how small it is, we can give it a name. And that name can become an object.

Principles:

- encapsulation
- message-sending
- late binding

History

- Object orientation is a way of looking at problems.
- It is the brainchild of Alan Kay (1960s), [Smalltalk](https://en.wikipedia.org/wiki/Smalltalk). Note: Ruby and it's influence Smalltalk allows for open classes and rewriting code, which is a mindshift change if you're coming from a compiled language (e.g. Java).
- It is an architectural style.
- "local retention and protection of hiding state-process". How does this relate to functions with no side effects?
- Polymorphism is a subset of "late binding". Our example uses symbolic names instead of object references.
- We leave out Classes, inheritance, types, exceptions, synchronous sends. But there is no mutable state. State modifications are applied all at once at the end.
- We tend to think of messages as bidirectional (there are return values).
- No management of instances.
- Cells and messages. If everything is a cell it becomes easy to extend and re-arrange cells.

Late binding

- Putting off the specifics as late as possible (only until they are needed). That makes me think of [lazy loading](https://en.wikipedia.org/wiki/Lazy_loading).
- Early bounding leaves very little room for ambiguity (e.g. procedural, imperative, you describe the algorithm step by step).
- I think of using [Gulp.js](https://gulpjs.com/) to manage Javascript assets. You tell it very explicitly to bundle the CSS, then tell it to bundle the images in one folder, then to bundle the Javascript in another folder.

```javascript
const { series } = require('gulp');

function transpile_my_javascripts(callback) {
  callback();
}

function bundle_my_css(callback) {
  callback();
}

exports.build = series(transpile_my_javascripts, bundle_my_css);

```

- Whereas with [Webpack](https://webpack.js.org/), you take a declarative approach. You tell Webpack what end state you want and it will get you there. 

```javascript
// webpack.config.js

const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js',
  },
};

```

### Barewords

- Moving variables to different scopes. Static/hardcoded values become: gobal/method arguments, property of a single instance (instance var), member of a composited object, property of the class (constant), constant in program wide module namespace.
- At every step we changed the scope of the named value and changed the greeting code (method interface) but underlying logic did not change.
- How can we change the source of the values used without modifying the greeting code? This is the goal because it shouldn't care what the new source of the values are.
- In Perl/Ruby, a bareword may look like a value and end up a named method. Is there any reason not to refer to a bareword instead of a constant? It makes it easier to move between variable scopes more easily.

What does it mean to send a Message?

- Alan Kay on [Messaging and Smalltalk](http://wiki.c2.com/?AlanKayOnMessaging)

> The key in making great and growable systems is much more to design how its modules communicate rather than what their internal properties and behaviors should be.

What makes for a good message definition in object-oriented code?

- Only the recipient of the message should decide what to do with it (it is late bound). Contrast this with a statically typed compiled language like C++ where object methods are early bound and very procedural.  
  - Javascript (which is dynamic) call also be treated procedurally.

```
var do_thing = object.do_thing
... later ...
do_thing()
```

- One of the possible outcomes of handling a message is for it to do nothing (no-op).
- One-way messages (Actor Model messaging)
  - In Erlang/Elixir, we have this concept. In other languages there is a heavy reliance on call-and-return functions.
  - Article on [east oriented code](https://www.saturnflyer.com/blog/the-4-rules-of-east-oriented-code-rule-1)
- Messages are important because they introduce space between objects. That space isn't real if the information inside the messages require objects to have a deep understanding of other objects.
  - You want to commoditize your messages for easy interchange.

Why do we care about this? Because early binding to a fixed method implementation can lead to bugs in our program.

### Methods vs Messages

- Methods are not first class objects in Ruby (as opposed to Python or Javascript) because Ruby is purely object oriented.
- There is a difference between calling a method and sending a message.
- When our code saved a direct reference to a method and then called it later, it missed out on any changes to that method which occurred between when the reference was saved and when the method was executed.
- Message = responsibility that an object may have
- Method = named, concrete piece of code that encodes one way a responsibility may be fulfilled.
- Message = layer of indirection on top of methods
- Method references can be in danger of going stale
- The goal is to pass messages between classes with no assumptions as to how those messages are to be handled.
- Ruby adheres to this guiding principle, so if you're wondering why ruby doesn't pass method references around more often, now you know.

### History of OOP

- [Dr. Alan Kay on the Meaning of “Object-Oriented Programming”](http://userpage.fu-berlin.de/~ram/pub/pub_jf47ht81Ht/doc_kay_oop_en)
  - I thought of objects being like biological cells and/or individual computers on a network, only able to communicate with messages.
  - I wanted to get rid of data.
  - The second phase of this was to finally understand LISP and then using this understanding to make much nicer and smaller and more powerful and more late bound understructures.
  - OOP to me means only messaging, local retention and protection and hiding of state-process, and extreme late-binding of all things. It can be done in Smalltalk and in LISP. There are possibly other systems in which this is possible, but I'm not aware of them.
  - Cheers, Alan
- "Dataless Programming"
  - Basic unit of computation == a whole computer. People who liked objects as "non data" were involved in the Arpanet/internet. The opposers tried to get by with RPC (remote procedure calls).
- [History of Smalltalk](http://worrydream.com/EarlyHistoryOfSmalltalk/)

### Capacitor session notes

- Remote pairing setup in the cloud. +1 to that.
- How far away can you move from Rails practices and it still be Rails?
  - Team maturity and team coherence
  - [Presenter](http://blog.jayfields.com/2007/03/rails-presenter-pattern.html), [Interactor](https://github.com/collectiveidea/interactor) and [Decorator](https://www.rubytapas.com/2014/04/25/episode-197-decorator/#tapas__decorator) Patterns
  - Use a [process](https://www.rubytapas.com/references/process-object-pattern/) object
