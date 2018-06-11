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
