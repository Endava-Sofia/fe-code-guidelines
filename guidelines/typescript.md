# Typescript guidelines and best practices

TypeScript Code Style Guidelines provide a concise set of conventions and best practices used to create consistent, maintainable code.

As projects grow in size and complexity, maintaining code quality and ensuring consistent practices becomes increasingly challenging. Defining and following a standard way to write TypeScript applications brings consistent codebase and faster development cycles.

## Naming Conventions

While it's often hard to find the best name, try optimize code for consistency and future reader by following conventions:

### Variables

- **Locals**\
  Camel case\
  `products`, `productsFiltered`

- **Booleans**\
  Prefixed with `is`, `has`, `should`, `will`, `can` etc.\
  `isDisabled`, `hasProduct`

- **Constants**\
  Capitalized\
  `PRODUCT_ID`

- **Object constants**\
  Singular, capitalized with const assertion and optionally satisfies type (if there is one).\

  ```ts
  const ORDER_STATUS = {
    pending: 'pending',
    fulfilled: 'fulfilled',
    error: 'error',
  } as const satisfies OrderStatus;
  ```

### Object and Class

Class and object literal properties are allowed in `non-camel-case` when quoted.

### Interfaces

PascalCase. Interface names should not be prefixed with `I` as that is already implied in their definition with the keyword `interface`.

### Functions

Camel case. Should contain a verb in the name to express the action and purpose of the function.\
`filterProductsByType`, `formatCurrency`

### Generics

A name starts with the capital letter T `TRequest`, `TFooBar`.
Avoid (popular convention) naming generics with one character `T`, `K` etc., the more variables we introduce, the easier it is to mistake them.

```ts
// ❌ Avoid naming generics with one character
const createPair = <T, K extends string>(first: T, second: K): [T, K] => {
  return [first, second];
};
const pair = createPair(1, 'a');

// ✅ Name starts with the capital letter T
const createPair = <TFirst, TSecond extends string>(first: TFirst, second: TSecond): [TFirst, TSecond] => {
  return [first, second];
};
const pair = createPair(1, 'a');
```

This can be enforced with a ESLint rule:

```ts
// .eslintrc.js
'@typescript-eslint/naming-convention': [
  'error',
  {
    selector: 'typeParameter',
    format: ['PascalCase'],
    custom: { regex: '^T[A-Z]', match: true },
  },
],
```

### Abbreviations & Acronyms

Treat acronyms as whole words, with capitalized first letter only.

```ts
// ❌ Avoid
const FAQList = ['qa-1', 'qa-2'];
const generateUserURL(params) => {...}

// ✅ Use
const FaqList = ['qa-1', 'qa-2'];
const generateUserUrl(params) => {...}
```

In favor of readability, strive to avoid abbreviations, unless they are widely accepted and necessary.

```ts
// ❌ Avoid
const GetWin(params) => {...}

// ✅ Use
const GetWindow(params) => {...}
```

### React Components

Pascal case
`ProductItem`, `ProductsPage`

### Prop Types

React component name following "Props" postfix
`[ComponentName]Props` - `ProductItemProps`, `ProductsPageProps`

## Exports

Use named exports in all code.

Good:

```ts
// Use named exports:
export class Foo { ... }
```

Bad:

```ts
// Do not use default exports:
export default class Foo { ... } // BAD!
```

Do not use default exports. This ensures that all imports follow a uniform pattern.

Why?

Default exports provide no canonical name, which makes central maintenance difficult with relatively little benefit to code owners, including potentially decreased readability:

```ts
import Foo from './bar';  // Legal.
import Bar from './bar';  // Also legal.
```

Named exports have the benefit of erroring when import statements try to import something that hasn't been declared. In `foo.ts`:

```ts
const foo = 'blah';
export default foo;
```

And in `bar.ts`:

```ts
import {fizz} from './foo';
```

Results in error `TS2614: Module '"./foo"' has no exported member 'fizz'`. While `bar.ts`:

```ts
import fizz from './foo';
```

Results in `fizz === foo`, which is probably unexpected and difficult to debug.

Additionally, default exports encourage people to put everything into one big object to namespace it all together:

```ts
export default class Foo {
  static SOME_CONSTANT = ...
  static someHelpfulFunction() { ... }
  ...
}
```

With the above pattern, we have file scope, which can be used as a namespace. We also have a perhaps needless second scope (the class `Foo`) that can be ambiguously used as both a type and a value in other files.

Instead, prefer use of file scope for namespacing, as well as named exports:

```ts
export const SOME_CONSTANT = ...
export function someHelpfulFunction()
export class Foo {
  // only class stuff here
}
```

## Export visibility

TypeScript does not support restricting the visibility for exported symbols. Only export symbols that are used outside of the module. Generally minimize the exported API surface of modules.

## Mutable exports

Regardless of technical support, mutable exports can create hard to understand and debug code, in particular with re-exports across multiple modules. One way to paraphrase this style point is that `export let` is not allowed.

```ts
export let foo = 3;
// In pure ES6, foo is mutable and importers will observe the value change after a second.
// In TS, if foo is re-exported by a second file, importers will not see the value change.
window.setTimeout(() => {
  foo = 4;
}, 1000 /* ms */);
```

If one needs to support externally accessible and mutable bindings, they should instead use explicit getter functions.

```ts
let foo = 3;

window.setTimeout(() => {
  foo = 4;
}, 1000 /* ms */);

// Use an explicit getter to access the mutable export.
export function getFoo() { return foo; };
```

## Import type

You may use `import type {...}` when you use the imported symbol only as a type. Use regular imports for values:

```ts
import type {Foo} from './foo';
import {Bar} from './foo';

import {type Foo, Bar} from './foo';
```

Why?

The TypeScript compiler automatically handles the distinction and does not insert runtime loads for type references. So why annotate type imports?

The TypeScript compiler can run in 2 modes:

- In development mode, we typically want quick iteration loops. The compiler transpiles to JavaScript without full type information. This is much faster, but requires import type in certain cases.
- In production mode, we want correctness. The compiler type checks everything and ensures import type is used correctly.

## Export type

Use `export type` when re-exporting a type, e.g.:

```ts
export type {AnInterface} from './foo';
```

Why?

- `export type` is useful to allow type re-exports in file-by-file transpilation. See [isolatedModules docs](https://www.typescriptlang.org/tsconfig#exports-of-non-value-identifiers).
- `export type` might also seem useful to avoid ever exporting a value symbol for an API. However it does not give guarantees, either: downstream code might still import an API through a different path. A better way to split & guarantee type vs value usages of an API is to actually split the symbols into e.g. `UserService` and `AjaxUserService`.
This is less error prone and also better communicates intent.

## Use modules not namespaces

TypeScript supports two methods to organize code: namespaces and modules, but namespaces are disallowed. That is, your code must refer to code in other files using imports and exports of the form `import {foo} from 'bar'`;

Your code must not use the `namespace Foo { ... }` construct. namespaces may only be used when required to interface with external, third party code. To semantically namespace your code, use separate files.

Code _must_ not use `require` (as in `import x = require('...');`) for imports. Use ES6 module syntax.

```ts
// Bad: do not use namespaces:
namespace Rocket {
  function launch() { ... }
}

// Bad: do not use <reference>
/// <reference path="..."/>

// Bad: do not use require()
import x = require('mydep');
```

NB: TypeScript `namespaces` used to be called internal modules and used to use the `module` keyword in the form `module Foo { ... }`. Don't use that either. Always use ES6 imports.

> This preference can be enforced with an existing ESLint rule in your project: <https://github.com/typescript-eslint/typescript-eslint/blob/main/packages/eslint-plugin/docs/rules/no-namespace.md>

## Extract Types

It's a good practice to extract all type definitions for each context to separate files named `types.ts` or `types.d.ts` for reusability, better code readability in the feature files and easier code management.

## Consistent Type Definitions

TypeScript provides two common ways to define an object type: `interface` and `type` (aka `type alias`). The two are generally very similar, and can often be used interchangeably. Using the same type declaration style consistently helps with code readability.

> This preference can be enforced with an existing ESLint rule in your project: <https://typescript-eslint.io/rules/consistent-type-definitions/>

## Consistent Type Assertions

Type assertions should not vary in style. Use `as` keyword for type assertions.

> This preference can be enforced with an existing ESLint rule in your project: <https://github.com/typescript-eslint/typescript-eslint/blob/main/packages/eslint-plugin/docs/rules/consistent-type-assertions.md>

## Consistent Array Type Definitions

TypeScript provides two equivalent ways to define an array type: `T[]` and `Array<T>`. The two styles are functionally equivalent. Using the same style consistently across your codebase makes it easier for developers to read and understand array types.

Always use `T[]` or `readonly T[]` syntax for all array types as it’s easier to read.

> This preference can be enforced with an existing ESLint rule in your project: <https://typescript-eslint.io/rules/array-type/>

## Enums

Don't duplicate `enum` values.

Bad:

```ts
enum E {
  A = 0,
  B = 0,
}

enum E {
  A = 'A',
  B = 'A',
}
```

Good:

```ts
enum E {
  A = 0,
  B = 1,
}

enum E {
  A = 'A',
  B = 'B',
}
```

> This preference can be enforced with an existing ESLint rule in your project: <https://typescript-eslint.io/rules/no-duplicate-enum-values/>

We also don’t recommend using string enum types - use string literal union types instead.

Why?

Union Types are simpler than Enums, as they don't require any additional runtime or compilation overhead. Instead of creating a separate object, Union Types define a new type that is the union of a set of literal types. This makes them more efficient in terms of both memory usage and file size.

Read more here: <https://medium.com/totally-typescript/why-typescript-enums-are-terrible-but-union-types-are-great-83324f571eba>

## Type Inference

Don't add types in places where TypeScript can infer them on its own:

Read more here: <https://www.tutorialsteacher.com/typescript/type-inference>

## Meaningless Void Operators

Don't add meaningless void operators.

Bad:

```ts
void (() => {})();

function foo() {}
void foo();
```

Good:

```ts
(() => {})();

function foo() {}
foo(); // nothing to discard

function bar(x: number) {
  void x; // discarding a number
  return 2;
}
void bar(); // discarding a number
```

Read more here: <https://github.com/typescript-eslint/typescript-eslint/blob/main/packages/eslint-plugin/docs/rules/no-meaningless-void-operator.md>

> This preference can be enforced with an existing ESLint rule in your project: <https://typescript-eslint.io/rules/no-meaningless-void-operator/>

## Redundant Type Constituents

Don't add redundant type constituents - Some types can override some other types ("constituents") in a union or intersection and or be overridden by some other types. TypeScript's set theory of types includes cases where a constituent type might be useless in the parent union or intersection.

Bad:

```ts
type UnionAny = any | 'foo';
type UnionUnknown = unknown | 'foo';
type UnionNever = never | 'foo';

type UnionBooleanLiteral = boolean | false;
type UnionNumberLiteral = number | 1;
type UnionStringLiteral = string | 'foo';

type IntersectionAny = any & 'foo';
type IntersectionUnknown = string & unknown;
type IntersectionNever = string | never;

type IntersectionBooleanLiteral = boolean & false;
type IntersectionNumberLiteral = number & 1;
type IntersectionStringLiteral = string & 'foo';
```

Good:

```ts
type UnionAny = any;
type UnionUnknown = unknown;
type UnionNever = never;

type UnionBooleanLiteral = boolean;
type UnionNumberLiteral = number;
type UnionStringLiteral = string;

type IntersectionAny = any;
type IntersectionUnknown = string;
type IntersectionNever = string;

type IntersectionBooleanLiteral = false;
type IntersectionNumberLiteral = 1;
type IntersectionStringLiteral = 'foo';
```

> This preference can be enforced with an existing ESLint rule in your project: <https://typescript-eslint.io/rules/no-redundant-type-constituents/>

## Type Alias vs Interface

TypeScript provides two common ways to define an object type: `interface` and `type` (aka `type alias`). The two are generally very similar, and can often be used interchangeably.

Use `type` when you might need a union or intersection:

```ts
type Foo = number | { someProperty: number }
```

Use `interface` when you want `extends` or `implements` or when dealing with classes in your project code e.g.

```ts
interface Foo {
  foo: string;
}

interface FooBar extends Foo {
  bar: string;
}

class X implements FooBar {
  foo: string;
  bar: string;
}
```

Always use `interface` for public API's definition when authoring a library or 3rd party ambient type definitions, as this allows a consumer to extend them via [declaration merging](https://www.typescriptlang.org/docs/handbook/declaration-merging.html) if some definitions are missing.

We recommend using `interface` for defining React Component Props and State, for consistency and because we usually have to extend other interfaces of React or other libraries.

## Null and Undefined

Use _truthy_ check for objects being `null` or `undefined`. Only exception is when dealing with an integer number that can be `0`, `null` or `undefined` in which case it's best to use an utility function to check such values for existence.

Bad:

```ts
if (error === null))
```

Good:

```ts
if (error)
```

Prefer not to use `null` or `undefined` for explicit unavailability. Reason is these values are commonly used to keep a consistent structure between values. In TypeScript you use types to denote the structure.

Bad:

```ts
let foo = { x: 123, y: undefined };
```

Good:

```ts
let foo: { x: number, y?: number } = { x:123 };
```

Use `null` where it's a part of the API or conventional. It’s conventional in Node.js e.g. `error` is `null` for NodeBack style callbacks.

Bad:

```ts
cb(undefined)
```

Good:

```ts
cb(null)
```

## Generic Types

Dynamic types can be achieved with the use of [generics](https://www.typescriptlang.org/docs/handbook/2/generics.html), allowing the developer to dynamically pass types:

```ts
interface HttpResponse<T> {
  status: number;
  statusText: string;
  data: T;
}

type UserResponse = HttpResponse<User>;
/* result: the same type as HttpResponse but with the User type used on the place of the generic
{
  status: number;
  statusText: string;
  data: User;
}
*/
```

Generics can also be used in functions:

```ts
function fetchUserData<T>(id: string): Promise<T> {
  return this.api.get<T>(`/users/${id}`);
}

const user = await fetchUserData<User>('test');
```

The constant `user` will have a type of `User`.

## Utility Types

Another way of creating types is by transforming existing types with the so-called [Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html). Here are a few examples:

```ts
interface User {
  uuid: string;
  firstName: string;
  lastName: string;
  email?: string;
}

type UtilTest1 = Partial<User>;
/* result: the same type but every field is optional
(the opposite utility type is Required<T>) */
interface Result1 {
  uuid?: string;
  firstName?: string;
  lastName?: string;
  email?: string;
}

type UtilTest2 = Omit<User, 'firstname' | 'lastName'>;
/* result: the same type but with omited properties */
interface Result2 {
  uuid: string;
  email?: string;
}

// Creating types from function types
type SomeFunction = (arg1: string, arg2: number) => boolean;

type UtilTest3 = Parameters<SomeFunction>
/* result: a tuple with the parameters of the function */
type Result3 = [arg1: string, arg2: number]

type UtilTest4 = ReturnType<SomeFunction>
/* result: the return type of the function */
type Result4 = boolean;

```

## Dealing with type uncertainties

There could be cases when we are not sure about the exact type of something. Developers who are used to writing vanilla JS are tempted to use `any`. However, it's highly discouraged because it defies the very purpose of TypeScript and can potentially cause issues that TypeScript is designed to avoid in the first place. It can also be forbidden using strict tsconfig and additional TypeScript ESLint rules like `@typescript-eslint/no-explicit-any` and `@typescript-eslint/no-unsafe-assignment`. So here are a few examples on how to deal with type uncertainties:

1. Simply use `unknown`. Just like `any`, the `unknown` type implies that the value can be of any type, but it's safer because it's not legal to do anything with it. So if you want to assign such value or do anything with it you need to rely on good old runtime type checking.
2. If you are sure that the value is an object you can avoid giving it a fully `unknown` type and instead use indexed object types `{ [key: string]: unknown }` or `Record<string, unknown>` (both do the same). This makes things a bit clearer because at least we know we are dealing with an object that has some properties. But since the properties themselves are `unknown` you need to fall back to example 1 when using them.
3. If you are sure that the value is an array you can use `unknown[]` or if the value is an array of objects - `Record<string, unknown>[]`.
4. If you know that the value can be one of multiple types but the actual type is dynamic and unknown at a given moment, use [union types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types) (for example `string | number`). To work with a union typed value you first need to [narrow down its type](https://www.typescriptlang.org/docs/handbook/2/narrowing.html).

```ts
function someMethod(arg1: Date | number): void {
  if (typeof arg1 === 'number') {
    // TypeScript recognises arg1 as a number here
    const test = arg1 - 500
  } else {
    // TypeScript recognises arg1 as a Date here
    const test = arg1.toISOString();
  }
}
```

5. If the value can be one of multiple types depending on its use case maybe you can also rely on [generics](#generics).

## Use string literal types

In cases where we have a string value, but we know it always has to follow a certain format we can use string literal types:

```ts
type OS = 'windows' | 'linux' | 'macos';
type Arch = 'x64' | 'x86';
type Build = `${OS}_${Arch}`; // example: 'windows_x86'

type ISODateString = `${number}-${number}-${number}T${number}:${number}:${number}`; // example: '2023-09-10T19:00:00'
```

## Read the TypeScript Handbook

In current days of front-end development, it's crucial to have a good understanding of TypeScript. While learning modern front-end frameworks, a developer also learns TypeScript but that's only to some extent.

Going through the [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html) on their official website will provide you with a very good knowledge and skills, starting from the basics and building up to the most advanced features of the language.

## Creating types from other types

Part of [the Handbook](#read-the-typescript-handbook) touches the topic of creating types from already existing ones. It's always worth trying some approaches for creating types which are dependent on others to avoid repeating the same properties and potential mistakes during the process.
