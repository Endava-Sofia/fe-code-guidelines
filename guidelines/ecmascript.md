- [ECMAScript guidelines and best practices](#ecmascript-guidelines-and-best-practices)
  - [Prefer using only `const` and `let` variables everywhere and not `var`](#prefer-using-only-const-and-let-variables-everywhere-and-not-var)
  - [Prefer object spread everywhere](#prefer-object-spread-everywhere)
  - [Prefer `array and object destructuring` for constants everywhere](#prefer-array-and-object-destructuring-for-constants-everywhere)
  - [Prefer `optional chaining` everywhere](#prefer-optional-chaining-everywhere)
  - [Prefer `nullish coalescing` everywhere](#prefer-nullish-coalescing-everywhere)
  - [Prefer using `template literal` syntax for string concatenation and for multi-line string values](#prefer-using-template-literal-syntax-for-string-concatenation-and-for-multi-line-string-values)
  - [Prefer using `includes` method for checking against multiple possible values](#prefer-using-includes-method-for-checking-against-multiple-possible-values)
  - [Prefer `async/await` everywhere (no await in loops though)](#prefer-asyncawait-everywhere-no-await-in-loops-though)
  - [Prefer object literal `property value shorthand` syntax](#prefer-object-literal-property-value-shorthand-syntax)
  - [Prefer using the newest ES string, array and object methods](#prefer-using-the-newest-es-string-array-and-object-methods)
  - [Prefer using `for...of` and `for...in` loops where appropriate](#prefer-using-forof-and-forin-loops-where-appropriate)
  - [Prefer using `filter`, `map`, `reduce`, etc. but not `forEach`](#prefer-using-filter-map-reduce-etc-but-not-foreach)
  - [Perefer using `arrow functions` everywhere where it’s applicable](#perefer-using-arrow-functions-everywhere-where-its-applicable)
  - [Prefer using rest parameters](#prefer-using-rest-parameters)
  - [Prefer using `Set`, `Map` and `Symbol` wherever these new data structures can be useful and simplify the code markup](#prefer-using-set-map-and-symbol-wherever-these-new-data-structures-can-be-useful-and-simplify-the-code-markup)
  - [Prefer using `numeric literals`](#prefer-using-numeric-literals)

# ECMAScript guidelines and best practices

It is recommended to use all available ES6-ES13 syntax features in order to keep the JavaScript/Typescript code leaner, more readable, maintainable and in some cases more performant.

## Prefer using only `const` and `let` variables everywhere and not `var`

Variables declared using the `var` keyword are either globally or functionally scoped, they do not support block-level scope. This means that if a variable is defined in a loop or in an if statement it can be accessed outside the block and accidentally redefined leading to a buggy program. As a general rule, you should avoid using the `var` keyword.

Using `const` when the variable's value is not meant to change accomplishes a few things:

- It tells others reading your code that you don't intend the value to change.

- It gives you a nice proactive error if you change the code so it writes to that variable. (A decent IDE can flag this up proactively, but if not, you'll get the error when running the code.) You can then make an informed, intentional decision: Should you change it to let, or did you not mean to change that variable's value in the first place?

- It gives a hint to the JavaScript engine's optimizer that you won't be changing that variable's value. While the engine can frequently work that out through code analysis, using const saves it the trouble. (Caveat: I have no idea whether this is actually useful to the JavaScript engine. It seems like it would be, but runtime code optimization is a very complicated and sometimes non-intuitive process).

> This preference can be enforced with an existing ESLint rule in your project: <https://eslint.org/docs/latest/rules/prefer-const>

## Prefer object spread everywhere

Introduced in ES2018, object spread is a declarative alternative which may perform better than the more dynamic, imperative `Object.assign`.

> This preference can be enforced with an existing ESLint rule in your project: <https://eslint.org/docs/latest/rules/prefer-object-spread>

## Prefer `array and object destructuring` for constants everywhere

With JavaScript ES6, a new syntax was added for creating variables from an array index or object property, called destructuring - a convenient way to extract values from data stored in (possibly nested) objects and arrays.

Bad:

```javascript
// With `array` enabled
var foo = array[0];
bar.baz = array[0];

// With `object` enabled
var foo = object.foo;
var foo = object['foo'];
```

Good:

```javascript
// With `array` enabled
var [ foo ] = array;
var foo = array[someIndex];
[bar.baz] = array;


// With `object` enabled
var { foo } = object;

var foo = object.bar;

let foo;
({ foo } = object);
```

> This preference can be enforced with an existing ESLint rule in your project: <https://eslint.org/docs/latest/rules/prefer-destructuring>

## Prefer `optional chaining` everywhere

`?.` optional chain expressions provide `undefined` if an object is `null` or `undefined`. Because the optional chain operator only chains when the property value is `null` or `undefined`, it is much safer than relying upon logical AND operator chaining `&&` which chains on any _truthy_ value. It is also often less code to use `?.` optional chaining than `&&` truthiness checks.

Bad:

```javascript
foo && foo.a && foo.a.b && foo.a.b.c;
foo && foo['a'] && foo['a'].b && foo['a'].b.c;
foo && foo.a && foo.a.b && foo.a.b.method && foo.a.b.method();

// With empty objects
(((foo || {}).a || {}).b || {}).c;
(((foo || {})['a'] || {}).b || {}).c;

// With negated `or`s
!foo || !foo.bar;
!foo || !foo[bar];
!foo || !foo.bar || !foo.bar.baz || !foo.bar.baz();

// this rule also supports converting chained strict nullish checks:
foo &&
  foo.a != null &&
  foo.a.b !== null &&
  foo.a.b.c != undefined &&
  foo.a.b.c.d !== undefined &&
  foo.a.b.c.d.e;
```

Good:

```javascript
foo?.a?.b?.c;
foo?.['a']?.b?.c;
foo?.a?.b?.method?.();

foo?.a?.b?.c?.d?.e;

!foo?.bar;
!foo?.[bar];
!foo?.bar?.baz?.();
```

> This preference can be enforced with an existing ESLint rule in your project: <https://github.com/typescript-eslint/typescript-eslint/blob/main/packages/eslint-plugin/docs/rules/prefer-optional-chain.md>

## Prefer `nullish coalescing` everywhere

The `??` nullish coalescing runtime operator allows providing a default value when dealing with `null` or `undefined`. Because the nullish coalescing operator only coalesces when the original value is `null` or `undefined`, it is much safer than relying upon logical OR operator chaining `||`, which coalesces on any _falsy_ value.

Bad:

```javascript
const foo: any = 'bar';
foo !== undefined && foo !== null ? foo : 'a string';
foo === undefined || foo === null ? 'a string' : foo;
foo == undefined ? 'a string' : foo;
foo == null ? 'a string' : foo;

const foo: string | undefined = 'bar';
foo !== undefined ? foo : 'a string';
foo === undefined ? 'a string' : foo;

const foo: string | null = 'bar';
foo !== null ? foo : 'a string';
foo === null ? 'a string' : foo;
```

Good:

```javascript
const foo: any = 'bar';
foo ?? 'a string';
foo ?? 'a string';
foo ?? 'a string';
foo ?? 'a string';

const foo: string | undefined = 'bar';
foo ?? 'a string';
foo ?? 'a string';

const foo: string | null = 'bar';
foo ?? 'a string';
foo ?? 'a string';
```

> This preference can be enforced with an existing ESLint rule in your project: <https://github.com/typescript-eslint/typescript-eslint/blob/main/packages/eslint-plugin/docs/rules/prefer-nullish-coalescing.md>

## Prefer using `template literal` syntax for string concatenation and for multi-line string values

In ES2015 (ES6), we can use template literals instead of string concatenation.

Bad:

```javascript
var str = "Hello, " + name + "!";
```

Good:

```javascript
var str = `Hello, ${name}!`;
```

> This preference can be enforced with an existing ESLint rule in your project: <https://eslint.org/docs/latest/rules/prefer-template>

## Prefer using `includes` method for checking against multiple possible values

Prior to ES2015, `Array#indexOf` and `String#indexOf` comparisons against `-1` were the standard ways to check whether a value exists in an array or string, respectively. Alternatives that are easier to read and write now exist: ES2015 added `String#includes` and ES2016 added `Array#includes`.

Having the current input data:

```javascript
const str: string;
const array: any[];
const readonlyArray: ReadonlyArray<any>;
const typedArray: UInt8Array;
const maybe: string;
const userDefined: {
  indexOf(x: any): number;
  includes(x: any): boolean;
};
```

Bad:

```javascript
str.indexOf(value) !== -1;
array.indexOf(value) !== -1;
readonlyArray.indexOf(value) === -1;
typedArray.indexOf(value) > -1;
maybe?.indexOf('') !== -1;
userDefined.indexOf(value) >= 0;

/example/.test(str);
```

Good:

```javascript
str.includes(value);
array.includes(value);
!readonlyArray.includes(value);
typedArray.includes(value);
maybe?.includes('');
userDefined.includes(value);

str.includes('example');
```

It's also a good idea to use `Array#includes` method when you have an `if` condition where you check if a value matches a multiple set of values instead of chaining multiple boolean conditions with `&&` logical operator.

> This preference can be enforced with an existing ESLint rule in your project: <https://github.com/typescript-eslint/typescript-eslint/blob/main/packages/eslint-plugin/docs/rules/prefer-includes.md>

## Prefer `async/await` everywhere (no await in loops though)

It is a way to write asynchronous code in JavaScript/TypeScript. Promises are one of the best additions to JavaScript because they make handling async code so much easier. Going from callbacks to promises feels like a massive upgrade, but there is something even better than promises and that is async/await. Async/await is an alternative syntax for promises that makes reading/writing async code even easier.

Bad:

```javascript
function setTimeoutPromise(delay) {
  return new Promise((resolve, reject) => {
    if (delay < 0) return reject("Delay must be greater than 0")

    setTimeout(() => {
      resolve(`You waited ${delay} milliseconds`)
    }, delay)
  })
}

setTimeoutPromise(250)
  .then(msg => {
    console.log(msg)
    console.log("First Timeout")
    return setTimeoutPromise(500)
  })
  .then(msg => {
    console.log(msg)
    console.log("Second Timeout")
  })
// Output:
// You waited 250 milliseconds
// First Timeout
// You waited 500 milliseconds
// Second Timeout
```

Good:

```javascript
function setTimeoutPromise(delay) {
  return new Promise((resolve, reject) => {
    if (delay < 0) return reject("Delay must be greater than 0")

    setTimeout(() => {
      resolve(`You waited ${delay} milliseconds`)
    }, delay)
  })
}

doStuff()
async function doStuff() {
  const msg1 = await setTimeoutPromise(250)
  console.log(msg1)
  console.log("First Timeout")

  const msg2 = await setTimeoutPromise(500)
  console.log(msg2)
  console.log("Second Timeout")
}
// Output:
// You waited 250 milliseconds
// First Timeout
// You waited 500 milliseconds
// Second Timeout
```

> This preference can be enforced with existing ESLint rules in your project: <https://github.com/eslint-community/eslint-plugin-promise/blob/main/docs/rules/prefer-await-to-then.md> and <https://eslint.org/docs/latest/rules/require-await>

## Prefer object literal `property value shorthand` syntax

ECMAScript 6 provides a concise form for defining object literal methods and properties. This syntax can make defining complex object literals much cleaner.

Bad:

```javascript
// properties
var foo = {
    x: x,
    y: y,
    z: z,
};

// methods
var foo = {
    a: function() {},
    b: function() {}
};
```

Good:

```javascript
// properties
var foo = {x, y, z};

// methods
var foo = {
    a() {},
    b() {}
};
```

> This preference can be enforced with an existing ESLint rule in your project: <https://eslint.org/docs/latest/rules/object-shorthand>

## Prefer using the newest ES string, array and object methods

This is recommended to do as long as these methods don't require additional polyfill scripts for the modern browsers that are to be supported - Safari, Edge, Firefox, Chrome for example.

## Prefer using `for...of` and `for...in` loops where appropriate

Many developers default to writing `for (let i = 0; i < ...` loops to iterate over arrays. However, in many of those arrays, the loop iterator variable (e.g. `i`) is only used to access the respective element of the array. In those cases, a `for-of` loop is easier to read and write.

Bad:

```javascript
declare const array: string[];

for (let i = 0; i < array.length; i++) {
  console.log(array[i]);
}
```

Good:

```javascript
declare const array: string[];

for (const x of array) {
  console.log(x);
}

for (let i = 0; i < array.length; i++) {
  // i is used, so for-of could not be used.
  console.log(i, array[i]);
}
```

The `for...in` statement iterates over all enumerable string properties of an object (ignoring properties keyed by symbols), including inherited enumerable properties.

Caveats of using `for...in`:

If you only want to consider properties attached to the object itself, and not its prototypes, you can use one of the following techniques:

```javascript
Object.keys(myObject)
Object.getOwnPropertyNames(myObject)
```

`Object.keys` will return a list of enumerable own string properties, while `Object.getOwnPropertyNames` will also contain non-enumerable ones.

Many JavaScript style guides and linters recommend against the use of `for...in`, because it iterates over the entire prototype chain which is rarely what one wants, and may be a confusion with the more widely-used `for...of` loop. `for...in` is most practically used for debugging purposes, being an easy way to check the properties of an object (by outputting to the console or otherwise). In situations where objects are used as ad hoc key-value pairs, `for...in` allows you check if any of those keys hold a particular value.

> This preference can be enforced with an existing ESLint rule in your project: <https://typescript-eslint.io/rules/prefer-for-of/>

## Prefer using `filter`, `map`, `reduce`, etc. but not `forEach`

`forEach` has known pitfalls that make it unsuitable in some situations that can be properly handled with `for` or `for..of`:

- callback function creates new context (can be addressed with arrow function)
- doesn't support iterators
- doesn't support generator `yield` and `async..await`
- doesn't provide a proper way to terminate a loop early with `break`
- it isn't immutable by nature. New value isn't provided for you automatically (like with `reduce` or `map`).

## Perefer using `arrow functions` everywhere where it’s applicable

Arrow functions can be an attractive alternative to function expressions for callbacks or function arguments.

For example, arrow functions are automatically bound to their surrounding scope/context. This provides an alternative to the pre-ES6 standard of explicitly binding function expressions to achieve similar behavior.

Additionally, arrow functions are:

- less verbose, and easier to reason about.
- bound lexically regardless of where or when they are invoked.

Bad:

```javascript
foo(function(a) { return a; });

foo(function() { return this.a; }.bind(this));
```

Good:

```javascript
foo(a => a);

foo(() => this.a);
```

> This preference can be enforced with an existing ESLint rule in your project: <https://eslint.org/docs/latest/rules/prefer-arrow-callback>

## Prefer using rest parameters

There are rest parameters in ES2015. We can use that feature for variadic functions instead of the arguments variable.

Bad:

```javascript
function foo() {
    console.log(arguments);
}

function foo(action) {
    var args = Array.prototype.slice.call(arguments, 1);
    action.apply(null, args);
}

function foo(action) {
    var args = [].slice.call(arguments, 1);
    action.apply(null, args);
}
```

Good:

```javascript
function foo(...args) {
    console.log(args);
}

function foo(action, ...args) {
    action.apply(null, args); // or `action(...args)`, related to the `prefer-spread` rule.
}

// Note: the implicit arguments can be overwritten.
function foo(arguments) {
    console.log(arguments); // This is the first argument.
}
function foo() {
    var arguments = 0;
    console.log(arguments); // This is a local variable.
}
```

> This preference can be enforced with an existing ESLint rule in your project: <https://eslint.org/docs/latest/rules/prefer-rest-params>

## Prefer using `Set`, `Map` and `Symbol` wherever these new data structures can be useful and simplify the code markup

Application of `Set` in JavaScript:

- Deleting duplicates element from an array
- Creating a unique array from the unique values of two arrays
- Union of two sets
- Intersection of two sets

Good:

```javascript

// Create an array containing duplicate items
const duplcateArray = [
    2, 3, 4, 8, 8, 9, 9, 4, 2, 
    3, 3, 4, 6, 7, 5, 32, 3, 4, 5
];
 
// Use Set and Spread Operator to remove 
// duplicate items from an array
console.log([...new Set(duplcateArray)]);
```

Use `Map` for dictionaries or hash maps where you have a variable number of entries, with frequent updates, whose keys might not be known at author time, such as an event emitter.

Before ES6, the only way to get a hash map is by creating an empty object.

Bad:

```javascript
const hashMap = {}
```

Using objects for hash maps can cause confusion and security hazards:

- _Unwanted inheritance_ -
  Upon creation, this object is no longer empty. Although hashMap is made with an empty object literal, it automatically inherits from Object.prototype. That's why we can invoke methods like hasOwnProperty, toString, constructor on hashMap even though we never explicitly defined those methods on the object.

  Because of prototypal inheritance, we now have two types of properties conflated: properties that live within the object itself, i.e. its own properties, and properties that live in the prototype chain, i.e. inherited properties. As a result, we need an additional check (e.g. hasOwnProperty) to make sure a given property is indeed user-provided, as opposed to inherited from the prototype.

  On top of that, because of how the property resolution mechanism works in JavaScript, any change to Object.prototype at runtime will cause a ripple effect in all objects. This opens the door for prototype pollution attacks, which can be a serious security issue for large JavaScript applications.

  Fortunately, we can work around this by using Object.create(null), which makes an object that inherits nothing from Object.prototype.

- _Name collisions_ - When an object's own properties have name collisions with ones on its prototype, it breaks expectations and thus crashes your program.
- _Sub-optimal ergonomics_ - Object doesn't provide adequate ergonomics to be used as a hash map. Many common tasks can't be intuitively performed (get size, iterate, clear, check property existence).

ES6 brings us `Map`. It is much more suited for a hash map use case.

Good:

```javascript
const hashMap = new Map();

hashMap.set('a', 1);
hashMap.set('b', 2);
hashMap.set('c', 3);
hashMap.get('a'); // 1
hashMap.set('a', 'ok');
hashMap.get('a'); // 'ok'
hashMap.size; // 3
hashMap.delete('b'); // removes b key/value
hashMap.clear() // empties map
```

`Symbols` are often used to add unique property keys to an object that won't collide with keys any other code might add to the object, and which are hidden from any mechanisms other code will typically use to access the object. That enables a form of weak encapsulation, or a weak form of information hiding.

## Prefer using `numeric literals`

The `parseInt()` and `Number.parseInt()` functions can be used to turn binary, octal, and hexadecimal strings into integers. As binary, octal, and hexadecimal literals are supported in ES6, this point encourages use of those numeric literals instead of `parseInt()` or `Number.parseInt()`.

Bad:

```javascript
parseInt("111110111", 2) === 503;
parseInt(`111110111`, 2) === 503;
parseInt("767", 8) === 503;
parseInt("1F7", 16) === 503;
Number.parseInt("111110111", 2) === 503;
Number.parseInt("767", 8) === 503;
Number.parseInt("1F7", 16) === 503;
```

Good:

```javascript
parseInt(1);
parseInt(1, 3);
Number.parseInt(1);
Number.parseInt(1, 3);

0b111110111 === 503;
0o767 === 503;
0x1F7 === 503;

a[parseInt](1,2);

parseInt(foo);
parseInt(foo, 2);
Number.parseInt(foo);
Number.parseInt(foo, 2);
```

> This preference can be enforced with an existing ESLint rule in your project: <https://eslint.org/docs/latest/rules/prefer-numeric-literals>
