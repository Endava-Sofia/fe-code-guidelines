# FE Code Style Guidelines aka Coding Convention

This repo contains a collection of `.md` documents with recommended rules set in place to ensure readability and manageability of code in medium and large codebases written in JavaScript/TypeScript.
Especially because JavaScript is more flexible in terms of syntax (dynamic typing, binding this, native object manipulation etc.), without a mutually agreed upon rules among developers, it will be near impossible to understand or to debug other's codes.

Abiding by the Coding Convention increases readability and decreases chances of errors or bugs that might affect the performance of the program/app.
For bigger projects, it is much more cost-efficient and reasonable to follow the coding conventions.

## Anti-Pattern

Anti-pattern is a set of patterns many developers habitually use, but is rejected for the sake of performance, debugging, maintenance, and readability.

The documents in this repo explain each anti-pattern with examples and provide suggestions for improvement.

## Static Analysis

JavaScript, compared to other languages, has a more flexible syntax. Such characteristic sometimes causes unintended problems.
For example, the flexible syntax creates accidental errors that are not, from syntactical perspective, bugs, and also leads to code which purpose is not initially transparent.
Because JavaScript does not have a compile stage, such errors are undetectable before actually running the program/app.

While adhering to the Coding Convention to increase readability and prevent anti-patterns is a temporary solution, it is still difficult for developers to ensure that they are following the Coding Convention by themselves.
In order to compensate for JavaScript’s shortcomings, Static Analysis using tools like ESLint/Sonarlint/SonarQube is used to automatically validate the Coding Convention and sniff out possible errors.

The documents in this repo also suggest existing ESLint rules that can be enforced in order to ensure developers follow the proposed Coding Convention while working on writing new code.

## Performance Optimization

Application performance optimization is important on both levels of the web and the application itself. Modern web applications have become very large and dense with features including API communication and complex UI with state management and routing.
Heavier web apps eventually lead to longer loading time and negatively affect the User Experience (UX).
Pinterest, for example, had high rates of users leaving the page due to long loading time (bounce rate), and through performance optimization, decreased the bounce rate and increased the revenue by 40%.
As the performance is clearly and directly linked to profit, it is critical to optimize the web application’s performance.

The documents in this repo serve to provide the readers with basic background knowledge of performance optimization, and to introduce various optimization methods by discussing the webpage loading and rendering stages separately.

## Dependency Management

JavaScript was created to quickly validate the user input without server communication. Although the first lines of JavaScript were distributed as means for simple tasks, recently, JavaScript has evolved into one of the major web development tools in the modern era.

As JavaScript functionalities increased, the code needed to be written grew larger and more complicated. Because of this, it was only natural for the code to get grouped together and separated according to the functionalities, but this resulted in a complex and dependent relationship.

The documents in this repo will illustrate the ever more growing complexity of JavaScript/TypeScript code and improved processes for managing the dependencies.

## ES5 to ES6+

ECMAScript (`ES` in ES5 and `ES2015` is abbreviation of ECMAScript) is a general purpose scripting language specification standard set by ECMA-262 of ECMA International.
The first build of ECMAScript was distributed in 1997, and it took them 12 years to get to the distribution of widely used `ES5` in 2009. Six years later, ECMA International released the sixth build, ECMAScript 2015, not ECMAScript 6 (from here on out `ES6`) as naturally predicted.
Previously it took them longer to publish each build, but in order to make the language more suitable to the fast-changing environment that is programming, it was published with the published year as a part of the name instead of the version number.

`ES6` addressed the issues raised in `ES5` and in older versions, and added new syntax that allowed for code to be written in a much cleaner form, thereby adding readability and maintainability.
Famous online JavaScript libraries (Lodash, React, Vue etc.) also followed suit and adopted `ES6`, allowing the new line of JavaScript libraries to be more easily accessible with the `ES6` platforms.

The documents in this repo serve to discuss the benefits of switching over to `ES6` and to help the readers better understand the newly published JavaScript libraries. Specifications made after `ES2015` are also included, but for the sake of convenience, will collectively be referred to as `ES6`.

## HTML/CSS/Sass

In web development, HTML forms the basic skeletal structure, and CSS decides how the markup language will be presented. Both are closely related to service performance and accessibility.
In other words, both HTML and CSS must be written well in order for all browsers to fully express the contents without significant loss.

The documents in this repo serve to help developers write consistent HTML/CSS/Sass code to facilitate cooperation and to minimize maintenance and expansion cost.

## Guidelines Table of Contents

- [Angular](./guidelines/angular.md#angular-guidelines-and-best-practicesanchors)
- [React](./guidelines/react.md#react-guidelines-and-best-practices)
- [TypeScript](./guidelines/typescript.md#typescript-guidelines-and-best-practices)
- [ECMAScript](./guidelines/ecmascript.md#ecmascript-guidelines-and-best-practices)
- [Sass](./guidelines/sass.md#sass-guidelines-and-best-practices)
- [CSS](./guidelines/css.md#css-guidelines-and-best-practices)
- [HTML](./guidelines/html.md#html-guidelines-and-best-practices)
- [Principles](./guidelines/principles.md#principles)
- [Code Readability](./guidelines/code.readability.md#code-readability-guidelines-and-best-practices)
