## JavaScript: The Good Parts by Douglas Crockford

### Grammar
- use `//` style comments exclusively because `/* */` style can occur in regex and make
a mess of things
- JS has a single number type: 64 bit floating point.
  - very large/small numbers can use scientific notation: `5.21e100`
  - `NaN` is a number value that is the results of an operation that cannot produce a normal result
    - not equal to any value, including itself
    - detect with `isNaN()`
- String literals can use single or double quotes
  - There is some disagreement about which is best, some big names use single quotes and some use double. Not very important as long as you are consistent (defined in style guide)
  - Strings are immutable (but String object is mutable, similar to Java)
- Inside a function, `var` defines the function's private variables
  - **code blocks `{ ... }` do not create new scope.** Variables should be defined at the top of the fuction, not in blocks
  - Falsy values: `false`, `null`, `undefined`, empty string, 0 (zero), `NaN`. All other values are truthy
  - Your typical control structures: `if/then`, `switch`, `while`, `for`, `do/while`

### Objects
- JavaScript simple types: numbers, strings, booleans (`true` and `false`), `null`, `undefined`
- Everything else is an object - a mutable keyed collection
  - container of properties, where a property has a name (any string) and a value (anything except `undefined`)

Object literal:
```JavaScript
var cruise = {
  vessel: "Queen Mary",
  number: 815
  departure: {
    port: "New York"
    date: "2018-05-05"
  }
  arrival: {
    port: "London"
    date: "2018-05-21"
  }
}
```
- Prefer dot notation `cruise.vessel` over array style `cruise["vessel"]`
- trying to access non-existant value results in `undefined` - use `||` to define a default
```JavaScript
var status = cruise.status || "unknown";
```
- trying to access a value from `undefined` will throw a `TypeError` - use && to short circuit
```JavaScript
var status = cruise.status && cruise.status.code; //undefined
```
- If you assign a property to an object and it does not already exist on that object, it will be added to the object.
- Objects are passed by reference
- All objects have a prototype from which it can inherit properties.
  - Objects created from object literals are linked to `Object.protoype`
  - When you create an object, you can specify what object should be its prototype
  - Prototype link is used only in retrieval. If you try to retrieve a property value from an object, and it's not there, JS attempts to retrieve the property from the prototype. If it's not there either, it goes up the rest of the prototype chain. Updates update only the actual object and do not touch any prototypes
- `delete` can be used to remove a property from an object (will not touch prototypes), this can sometimes allow prototype properties to "shine through"

Possible solution to global variable problem: create one single global object and use it as a container - creating a kind of namespace

### Functions
Functions are objects that are linked to `Function.protoype`. They can be used like any other value, stored in variables or arrays, returned from functions, etc. They are special because they can be *invoked*

Function Literal:
```JavaScript
var add = function(a, b) {
  return a + b
};
```

Functions receives 2 addition params beside the obvious.
1. `this` - value is determined by "invocation pattern"
2. `arguments`

If you pass too few params into a function, the unfilled params will be `undefined`. If you pass too many params into a function, the extra arguments will be ignored.

#### Method Invocation Pattern
**method**: when a function is stored as a property of an object

```JavaScript
var myObject = {
  value: 0,
  myFunction: function(replacement) {
    this.value = replacement;
  };
}

myObject.myFunction(1);
```
In this context, `myFunction` is able to access `myObject` using `this` and can read or modify it.

#### Function Invocation Pattern
Just a plain old function that is not the property of an object.

```JavaScript
var sum = add(3, 5);
```
In this context, `this` is bound to the **global object** which was a really bad idea.

The problem is that this prevents you from accessing the calling function's `this` when you want to do something. There is a workaround:

```JavaScript
myObject.double = function () {
  var that = this; //inner function will be able to access "that"

  var helper = function () {
    that.value = add(that.value, that.value);
  };

  helper();
};

myObject.double();
```
