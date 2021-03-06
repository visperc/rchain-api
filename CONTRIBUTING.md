# Code Conventions and Design Notes

All contributions should pass these checks (as noted in
`.travis.yml`):

```yaml
  - npm test
  - npm run flow-check
  - npm run lint
```

The `test` check is conventional unit tests.


## Static Typechecking: flow

We use [flow](https://flow.org/) for static typing. The `npm run
flow-check` script does a complete check and `npm run flow-status`
does an incremental check.


## Code Style: airbnb

We follow the [Airbnb JavaScript Style Guide][asg], mostly. Use `npm
run lint`.  See `.eslitrc.json` for additional details.

[asg]: https://github.com/airbnb/javascript#readme


## Object capability (ocap) discipline

In order to supporting robust composition and cooperation without
vulnerability, code in this project should adhere to [object
capability discipline][ocap].

  - **Memory safety and encapsulation**
    - There is no way to get a reference to an object except by
      creating one or being given one at creation or via a message; no
      casting integers to pointers, for example. _JavaScript is safe
      in this way._

      From outside an object, there is no way to access the internal
      state of the object without the object's consent (where consent
      is expressed by responding to messages). _We use `def` (aka
      `Object.freeze`) and closures rather than properties on `this`
      to achieve this._

  - **Primitive effects only via references**
    - The only way an object can affect the world outside itself is
      via references to other objects. All primitives for interacting
      with the external world are embodied by primitive objects and
      **anything globally accessible is immutable data**. There must be
      no `open(filename)` function in the global namespace, nor may
      such a function be imported. _It takes some discipline to use
      modules in node.js in this way.  We use a convention
      of only accessing ambient authority inside `if (require.main ==
      module) { ... }`._

[ocap]: http://erights.org/elib/capability/ode/ode-capabilities.html


## Protobuf Encoding: protobuf.js
All of our protobuf encoding and decoding is done using [protobuf.js](https://github.com/dcodeIO/protobuf.js)

![protobuf.js diagram](https://camo.githubusercontent.com/f090df881cc6c82ecb7c5d09c9fad550fdfd153e/687474703a2f2f64636f64652e696f2f70726f746f6275662e6a732f746f6f6c7365742e737667)

##  Struggles with extracting API doc

We don't use classes (TODO: cite explanation as to why not)
but neither of the relevant recipies seem to work:

> Many libraries and frameworks have special 'class constructor
> methods' that accept an object as an input and return a class with
> that object's properties as prototype properties.

https://github.com/documentationjs/documentation/blob/master/docs/RECIPES.md#class-factories-using-lends


We'd like to use these scripts in our `package.json`:

    "doc": "node ./node_modules/.bin/documentation build --github rnodeAPI.js -f html -o docs",
    "doc-watch": "node ./node_modules/.bin/documentation serve --watch --github rnodeAPI.js"
