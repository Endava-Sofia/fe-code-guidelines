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
