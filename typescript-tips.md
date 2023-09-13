# Tips and good practices when using TypeScript

## Read the TypeScript Handbook

In current days of front-end development, it's crucial to have a good understanding of TypeScript. While learning modern front-end frameworks, a developer also learns TypeScript but that's only to some extent. Going through the [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html) on their official website will provide you with a very good knowledge and skills, starting from the basics and building up to the most advanced features of the language.

## Creating types from other types

Part of [the Handbook](#read-the-typescript-handbook) touches the topic of creating types from already existing ones. It's always worth trying some approaches for creating types which are dependent on others to avoid repeating the same properties and potential mistakes drung the process.

### Extending other interfaces

TypeScript interfaces can extend other interfaces in a similar way to a class:

```ts
interface Item {
  id: string;
  name: string;
}

// ❌
interface SelectableItem {
  id: string;
  name: string;
  selected: boolean;
}

// ✅
interface SelectableItem extends Item {
  selected: boolean;
}
```

Extending an existing interface makes code cleaner because there's less writing and it's easy to track the parent interface. It's also safer for a couple of reasons:

- Prevents typos that could have been introduced during duplication.
- Prevents wrong property types that could have been introduced during duplication.
- Prevents the need to update the child interface everytime a new property is added, removed or changed at the parent interface.

### Utility types

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

## Generics

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

## Decide between using an Enum or a union type of values

Every now and then you have to deal with a value which can only be one of limited set of values. In strictly typed languages like Java and C you have Enums. While enums are not feature of JavaScript, they certainly are a [feature of TypeScript](https://www.typescriptlang.org/docs/handbook/enums.html). But before jumping straight to an Enum usage only, you need to consider if for a given case it's worth doing an Enum or just use union type of values. The following example shows the differences:

```ts
// Enum
enum OperatingSystem {
  WINDOWS = 'Windows',
  LINUX = 'Linux',
  MAC_OS = 'MacOS',
}
const os: OperatingSystem = OperatingSystem.WINDOWS;

// Union type of values
type OperatingSystem = 'Windows' | 'Linux' | 'MacOS';
const os: OperatingSystem = 'Linux';
```

From type checking perspective, both cases do the exact same job - you can only assign either of the pre-defined values and any other value is illegal. But when it comes to compiling to JavaScript this is where the differences come. `Enums` are essentially values and they are compiled into the JavaScript code. Here's how the `OperatingSystem` enum looks like after the TypeScript compiles it to plain JavaScript:

```js
var OperatingSystem;
(function (OperatingSystem) {
  OperatingSystem["Windows"] = "Windows";
  OperatingSystem["Linux"] = "Linux";
  OperatingSystem["MacOS"] = "MacOS";
})(OperatingSystem || (OperatingSystem = {}));
```

As for union types - just like other types and interfaces, they don't make it to the JavaScript code. Their only purpose is for type checking by the code editor and the compiler. Both approaches have good and bad use cases and it's up to the developer to decide what is more suitable for each case.
