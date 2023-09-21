# ECMAScript guidelines and best practices

It is recommended to use all available ES6-ES13 syntax features in order to keep the JavaScript/Typescript code leaner, more readable, maintainable and in some cases more performant.

## Prefer using only `const` and `let` variables everywhere and not `var`

Variables declared using the `var` keyword are either globally or functionally scoped, they do not support block-level scope. This means that if a variable is defined in a loop or in an if statement it can be accessed outside the block and accidentally redefined leading to a buggy program. As a general rule, you should avoid using the `var` keyword.

Using `const` when the variable's value is not meant to change accomplishes a few things:

- It tells others reading your code that you don't intend the value to change.

- It gives you a nice proactive error if you change the code so it writes to that variable. (A decent IDE can flag this up proactively, but if not, you'll get the error when running the code.) You can then make an informed, intentional decision: Should you change it to let, or did you not mean to change that variable's value in the first place?

- It gives a hint to the JavaScript engine's optimizer that you won't be changing that variable's value. While the engine can frequently work that out through code analysis, using const saves it the trouble. (Caveat: I have no idea whether this is actually useful to the JavaScript engine. It seems like it would be, but runtime code optimization is a very complicated and sometimes non-intuitive process).

> This preference can be enforced with an existing ESLint rule in your project

## Prefer object spread everywhere

Introduced in ES2018, object spread is a declarative alternative which may perform better than the more dynamic, imperative `Object.assign`.

> This preference can be enforced with an existing ESLint rule in your project

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

> This preference can be enforced with an existing ESLint rule in your project

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

> This preference can be enforced with an existing ESLint rule in your project

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

> This preference can be enforced with an existing ESLint rule in your project

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

> This preference can be enforced with an existing ESLint rule in your project

## Prefer using `includes` method for checking against multiple possible values

Prior to ES2015, `Array#indexOf` and `String#indexOf` comparisons against `-1` were the standard ways to check whether a value exists in an array or string, respectively. Alternatives that are easier to read and write now exist: ES2015 added `String#includes` and ES2016 added `Array#includes`.

Bad:

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
const str: string;
const array: any[];
const readonlyArray: ReadonlyArray<any>;
const typedArray: UInt8Array;
const maybe: string;
const userDefined: {
  indexOf(x: any): number;
  includes(x: any): boolean;
};

str.includes(value);
array.includes(value);
!readonlyArray.includes(value);
typedArray.includes(value);
maybe?.includes('');
userDefined.includes(value);

str.includes('example');

// The two methods have different parameters.
declare const mismatchExample: {
  indexOf(x: unknown, fromIndex?: number): number;
  includes(x: unknown): boolean;
};
mismatchExample.indexOf(value) >= 0;
```

It's also a good idea to use `Array#includes` method when you have an `if` condition where you check if a value matches a multiple set of values instead of chaining multiple boolean conditions with `&&` logical operator.

> This preference can be enforced with an existing ESLint rule in your project

## Prefer `async/await` everywhere (no await in loops though)

It is a way to write asynchronous code in JavaScript/TypeScript. Promises are one of the best additions to JavaScript because they make handling async code so much easier. Going from callbacks to promises feels like a massive upgrade, but there is something even better than promises and that is async/await. Async/await is an alternative syntax for promises that makes reading/writing async code even easier.

> This preference can be enforced with an existing ESLint rule in your project
