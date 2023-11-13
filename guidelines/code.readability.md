- [Code readability guidelines and best practices](#code-readability-guidelines-and-best-practices)
  - [11 principles of readable code](#11-principles-of-readable-code)
  - [Writing comments](#writing-comments)
  - [Naming Considerations](#naming-considerations)
  - [Code Formatting Rules for Readability and Maintainability](#code-formatting-rules-for-readability-and-maintainability)

# Code readability guidelines and best practices

Readable code is code that clearly communicates its intent to the reader. While it’s easier said than done and often a challenge for inexperienced developers, creating readable code is as important as solving any software problem — this is why it’s often one of the first techniques a developer learns on the job.

Code readability has two key requirements: The first is that the code must produce the expected result, and the second is that the code should be easy for other developers to read. Messy code is like a messy coffee cup; if it’s not clean, others won’t choose to clean it themselves simply so they’re able to use it!

Code readability is fundamental. Think about it: how can you fix something that you struggle to read?
Poorly written code can cause a variety of issues. For example, a developer who writes detailed code at the beginning of a system development process may realize a year later that they don’t understand their own code and will be left having to re-teach themselves their own system’s technicalities.
Alternatively, when code is easy to read, it’s always easy to understand, debug, maintain, and extend!

Good writing is clear thinking made visible. Coding is similar. When code is too complex, it’s hard to test and can lose all of its meaning for other developers. By keeping things tight and consistent, you can focus on what the code actually does rather than explaining it.

## 11 principles of readable code

- **Structure your code**

  Well-structured code requires an understanding of design patterns, proper thinking, and plenty of experience. When code is well structured, it has a clear purpose.
  All classes and methods should be clear and easily understood. Ask yourself: is the codebase easy to navigate? Remember that formatting should be consistent across classes!

- **Test your code**

  Complete manual and automated tests to ensure your code is readable. When you proactively test your code, it can be modified quickly and without fear of breaking things.
  Restructuring existing code is also risky without the necessary testing. However, with tests, you won’t need to worry about making large refactors in the future.
  If you’re a new developer, get into the habit of writing unit tests for your code to make sure it works correctly, and to help detect and protect against future bugs.

- **Ask a coworker to explain the code to you**

  Ask your developer teammates to read your code and give you feedback. Encourage your colleagues to ask questions if something doesn’t make sense.
  If your code is readable, your feedback givers should ask little to no clarifying questions. At the end of the feedback session, ask your teammates if they understood the intent of what you wrote and have them explain it back to you.
  Don’t get discouraged if there’s any misunderstanding. Remember that questions will only lead to opportunities to make your code more readable.

- **Simplify your code**

  The great painter Leonardo da Vinci once said that “simplicity is the ultimate sophistication.”
  Coding is a creative craft that is no different! Aim to make your code elegant - simple to read, orderly, and easy to understand.
  Don’t try to over-complicate things with elaborate tricks. Keep your classes a reasonable size and most of your functions short.

- **Align your code**

  It’s simple: aligning your code looks better! Aligning your code helps you present your hard work beautifully and makes it much easier to read.
  It may seem silly, but the aesthetics of code are often as important as the code itself. If your code isn’t aligned, you’ll need to put in extra effort when refactoring as well.
  Many developers better format their code by keeping it aligned to the left. Since we read from left to right, more of the code will be visible when formatted this way, and you may even be able to avoid the need to scroll the code editor when reading through your code horizontally.

- **Stick to a maximum of 80 characters per line**

  When you’re reading a newspaper, articles are arranged in many columns because it’s easier to read.
  Your code is no different. Limit your line length to around 80 characters.
  If your code is too wide, it may be difficult to look at on a small device, it will be difficult to compare changes to another file side by side, and it will definitely be difficult to present to your manager and teammates, or at meetings, conferences, and events.
  
- **Name the code based on variables that explain themselves**

  A big aspect of making your code easy to read lies in the name you choose. The name of your code should clearly describe its purpose.
  Name your variables, function, and methods by what they do or what they are. For example, if your variable gets something, include “get” in its name.
  If you can’t name the method or function, the function may be too complicated. If you can, go back and break complicated functions into smaller functions to simplify.

- **Ensure it has a single responsibility**

  The single responsibility principle means that every building block does one thing and one thing only. All building blocks, including methods, variables, and classes, follow this rule so it’s easy for the person reading the code to understand their responsibility.
  When your code follows this principle, it’s easily maintainable and scalable. When concerns are separated, you can focus on what features you actually want to work on rather than having to navigate interconnected lines of code.

- **Explain the _why_ and code the _what_**

  Comments shouldn’t tell us what we already know. They should tell us _why_ things are happening.
  You should be able to write no more than a one-line comment explaining what the code is doing.
  If you can’t, rewrite the code until it is more readable. If the code is obvious, you can avoid comments altogether. The code should be able to speak for itself in many cases.

- **Follow a style guide**

  Follow your language’s style guide if there is one! House rules keep everyone on the same page.
  Companies like Google swear by style guides to keep tens of thousands of programmers consistent. Style guides are composed of two parts: rules and guidance. Rules are mandated with few exceptions, and some companies enforce them throughout the entire organization.
  Guidance simply refers to recommended strategies and practices that allow for more leeway. When you follow a style guide, you may be better able to focus on what’s getting done rather than how it’s coded.

- **Use plenty of documentation**

  As you become a pro at writing readable code, document your progress. This way, you’ll always have an extensive knowledge base at your fingertips!

## Writing comments

The most important concept here is that the code itself is the primary means of commenting and documentation. Within 100 or 1000 lines of code, there is a wealth of opportunity to embed your intended meaning and use context to communicate with your audience.
Adding more supporting literature to compensate for confusing code usually just makes it more tedious and difficult to work with the code. Four principal mechanisms govern clear code communication: naming, structure, context, and comments.

Use comments only to add context or explain choices that cannot be expressed through thoughtful naming, structure or the context of operations. Keep in mind that comments will most commonly be used by another developer, user, or yourself down the line.

**Formatting comments** - please add an empty space after the opening (`//` and `/`) and before the closing comment (`/`) symbols for multi-line JS comments. For multi-line comments please start each new line with an empty space.

This is a bad TODO comment:

```javascript
//TODO: make it work.
```

This is a good TODO comment:

```javascript
/* 
  TODO: (@David) implement 2-step predictor-corrector integrator for stability. Shocks
  develop when using Adams-Bashforth integration.
*/
```

Keep in mind there is a cost to adding comments - just as with the source code, comments require maintenance. There’s nothing more frustrating than a misleading, outdated comment that doesn’t match up with what the code is actually doing.
It's also good practice to add an expiration date to all TODO comments so that they can be removed if they become stale and to force the dev team to act on it in the near future if it’s important - read more about this practice here -
<https://github.com/sindresorhus/eslint-plugin-unicorn/blob/main/docs/rules/expiring-todo-comments.md>.

When using `eslint-disable` or `@ts-ignore` comments to disable an ESLint or TypeScript compiler error for a particular line of code when that is unavoidable then it's good practice to also leave a description of the reason for this in the same comment line. This can be enforced with a combination of existing ESLint rules:

```javascript
// .eslintrc.js
...
"@typescript-eslint/ban-ts-comment": [
  "error",
  {
    "ts-expect-error": { descriptionFormat: "^ -- .+$" },
    "ts-ignore": { descriptionFormat: "^ -- .+$" },
    "ts-nocheck": { descriptionFormat: "^ -- .+$" },
    "ts-check": false,
  },
],
"eslint-comments/require-description": "error",
...
```

## Naming Considerations

- Trailing underscores are forbidden to be used in names everywhere as they decrease code readability.
- Leading underscores are forbidden to be used in all variable names as they decrease code readability.
- Class and object literal properties are allowed in non-camel-case when quoted.
- Global state action creator functions and normal js/ts function names should start with a verb e.g. `getUsers`.

## Code Formatting Rules for Readability and Maintainability

- Add an empty padding line before all expect statements in unit test cases and group them together if possible.

  This rule can be enforced with an existing ESLint rule from the following plugin package:

  <https://www.npmjs.com/package/eslint-plugin-jest-formatting>.

- Add an empty padding line before default case of a switch statement and make sure default switch cases are placed at the end of the switch statement.

  These rules can be enforced with exsting ESLint rules:

  <https://eslint.org/docs/latest/rules/default-case-last>,\
  <https://eslint.org/docs/latest/rules/default-case>.

- Add an empty padding line before the following statements: `try`, `return`, `if`, `switch`, `throw`, `while`, `for`, `export`, `do`, `continue`, `break`, `class`.

  This rule can be enforced with an existing ESLint rule:

  ```javascript
  // .eslintrc.js
  ...
  'padding-line-between-statements': [
    'error',
    { blankLine: 'always', prev: 'directive', next: '*' },
    { blankLine: 'any', prev: 'directive', next: 'directive' },
    { blankLine: 'always', prev: ['default'], next: '*' },
    {
      blankLine: 'always',
      prev: '*',
      next: [
        'try',
        'return',
        'if',
        'switch',
        'throw',
        'while',
        'for',
        'export',
        'do',
        'continue',
        'class'
      ]
    },
    { blankLine: 'any', prev: 'export', next: 'export' }
  ],
  ...
  ```

- Add an empty padding line after all of the import statements at the top of the file.

  This rule can be enforced with the existing ESLint rule:

  <https://eslint.org/docs/latest/rules/padding-line-between-statements>.

- Don't open a function body with `{}` for arrow functions that don't need one (they directly return a result statement or return based on a ternary operator).

  This rule can be enforced with the existing ESLint rule:

  <https://eslint.org/docs/latest/rules/arrow-body-style>.
  
- Don't nest ternary operators.

  This rule can be enforced with the existing ESLint rule:\
  <https://eslint.org/docs/latest/rules/no-nested-ternary>.
  
- Don't write useless return, catch, concat statements and escapes of string and regex chars.

  These rules can be enforced with the existing ESLint rules:

  <https://eslint.org/docs/latest/rules/no-useless-return>,\
  <https://eslint.org/docs/latest/rules/no-useless-catch>,\
  <https://eslint.org/docs/latest/rules/no-useless-escape>,\
  <https://eslint.org/docs/latest/rules/no-useless-concat>.

- Don't leave `debugger`, `alert` or `console.log` calls in the code.

  This rule can be enforced with the existing ESLint rules:

  <https://eslint.org/docs/latest/rules/no-debugger>,\
  <https://eslint.org/docs/latest/rules/no-alert>,\
  <https://eslint.org/docs/latest/rules/no-console>.

- Don't reassign function parameters in order to avoid causing unwanted side-effects in the code.

- Don't modify function parameters that are passed by reference (objects, arrays, etc.) but rather modify and return copies of them in order to not cause unwanted side-effects in the code.

- Don't redeclare variables in order to avoid causing unwanted side-effects in the code.

  This rule can be enforced with the existing ESLint rule:
  
  <https://eslint.org/docs/latest/rules/no-redeclare>.

- Don't use global variables unless it is absolutely necessary (for example this is the only way to configure some third-party library) for better code maintainability and to avoid causing unwanted side-effects in the code.

- Don't leave unused expressions or variables in the code.

  This rule can be enforced with the existing ESLint rule:
  
  <https://eslint.org/docs/latest/rules/no-unused-expressions>.

- Don't leave multiple unnecessary spaces in the code.

  This rule can be enforced with the existing ESLint rule:
  
  <https://eslint.org/docs/latest/rules/no-multi-spaces>.

- Don't write useless else clauses (when you have return statements in them for example).

  This rule can be enforced with the existing ESLint rule:
  
  <https://eslint.org/docs/latest/rules/no-else-return>.

- Avoid using `await` in loops as it can cause performance issues.

  This rule can be enforced with the existing ESLint rule:
  
  <https://eslint.org/docs/latest/rules/no-await-in-loop>.

- Keep only one `var`/`let`/`const` declaration per line.

  This rule can be enforced with the existing ESLint rule:
  
  <https://eslint.org/docs/latest/rules/one-var-declaration-per-line>.

- Avoid using `undefined` explicitly (e.g. initializing variables with a value of undefined explicitly).

  This rule can be enforced with the existing ESLint rules:
  
  <https://eslint.org/docs/latest/rules/no-undefined>,
  <https://eslint.org/docs/latest/rules/no-undef>.

- Don't leave empty functions.

  This rule can be enforced with the existing ESLint rule:
  
  <https://eslint.org/docs/latest/rules/no-empty-function>.

- Don't duplicate class members.

  This rule can be enforced with the existing ESLint rule:
  
  <https://eslint.org/docs/latest/rules/no-dupe-class-members>.

- Don't add useless empty exports.

  This rule can be enforced with the existing ESLint rule:
  
  <https://github.com/typescript-eslint/typescript-eslint/blob/main/packages/eslint-plugin/docs/rules/no-useless-empty-export.md>.

- Add parameters with a default value at the end of the function's parameter list to avoid breaking changes in the code.

  This rule can be enforced with the existing ESLint rule:
  
  <https://eslint.org/docs/latest/rules/default-param-last>.

- Don't add unreachable code.

  This rule can be enforced with the existing ESLint rule:

  <https://eslint.org/docs/latest/rules/no-unreachable>.

- Write fewer longer unit tests.

  More info here:
  
  <https://kentcdodds.com/blog/write-fewer-longer-tests>.
