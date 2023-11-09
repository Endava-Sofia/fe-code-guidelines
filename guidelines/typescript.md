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

  ```javascript
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

```javascript
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

```javascript
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

```javascript
// ❌ Avoid
const FAQList = ['qa-1', 'qa-2'];
const generateUserURL(params) => {...}

// ✅ Use
const FaqList = ['qa-1', 'qa-2'];
const generateUserUrl(params) => {...}
```

In favor of readability, strive to avoid abbreviations, unless they are widely accepted and necessary.

```javascript
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

```javascript
// Use named exports:
export class Foo { ... }
```

Bad:

```javascript
// Do not use default exports:
export default class Foo { ... } // BAD!
```

Do not use default exports. This ensures that all imports follow a uniform pattern.

Why?

Default exports provide no canonical name, which makes central maintenance difficult with relatively little benefit to code owners, including potentially decreased readability:

```javascript
import Foo from './bar';  // Legal.
import Bar from './bar';  // Also legal.
```

Named exports have the benefit of erroring when import statements try to import something that hasn't been declared. In `foo.ts`:

```javascript
const foo = 'blah';
export default foo;
```

And in `bar.ts`:

```javascript
import {fizz} from './foo';
```

Results in error `TS2614: Module '"./foo"' has no exported member 'fizz'`. While `bar.ts`:

```javascript
import fizz from './foo';
```

Results in `fizz === foo`, which is probably unexpected and difficult to debug.

Additionally, default exports encourage people to put everything into one big object to namespace it all together:

```javascript
export default class Foo {
  static SOME_CONSTANT = ...
  static someHelpfulFunction() { ... }
  ...
}
```

With the above pattern, we have file scope, which can be used as a namespace. We also have a perhaps needless second scope (the class `Foo`) that can be ambiguously used as both a type and a value in other files.

Instead, prefer use of file scope for namespacing, as well as named exports:

```javascript
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

```javascript


export let foo = 3;
// In pure ES6, foo is mutable and importers will observe the value change after a second.
// In TS, if foo is re-exported by a second file, importers will not see the value change.
window.setTimeout(() => {
  foo = 4;
}, 1000 /* ms */);
```

If one needs to support externally accessible and mutable bindings, they should instead use explicit getter functions.

```javascript
let foo = 3;

window.setTimeout(() => {
  foo = 4;
}, 1000 /* ms */);

// Use an explicit getter to access the mutable export.
export function getFoo() { return foo; };
```

## Import type

You may use `import type {...}` when you use the imported symbol only as a type. Use regular imports for values:

```javascript
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

```javascript
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

```javascript
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

> This preference can be enforced with an existing ESLint rule in your project: <https://typescript-eslint.io/rules/no-duplicate-enum-values/>

We also don’t recommend using string enum types -
use string literal union types instead.

Why?

Union Types are simpler than Enums, as they don't require any additional runtime or compilation overhead. Instead of creating a separate object, Union Types define a new type that is the union of a set of literal types. This makes them more efficient in terms of both memory usage and file size.

Read more here: <https://medium.com/totally-typescript/why-typescript-enums-are-terrible-but-union-types-are-great-83324f571eba>

## Type Inference

Don't add types in places where TypeScript can infer them on its own:

Read more here: <https://www.tutorialsteacher.com/typescript/type-inference>

## Meaningless Void Operators

Don't add meaningless void operators.

Read more here: <https://github.com/typescripteslint/typescript-eslint/blob/main/packages/eslint-plugin/docs/rulesno-meaninglessvoid-operator.md>

> This preference can be enforced with an existing ESLint rule in your project: <https://typescript-eslint.io/rules/no-meaningless-void-operator/>
