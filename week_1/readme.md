Week 1
March 29 2021

What does it mean to be object-oriented?

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

- Whereas with Webpack, you take a declarative approach. You tell Webpack what end state you want and it will get you there. 

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
