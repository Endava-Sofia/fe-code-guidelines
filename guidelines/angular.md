### Table of Contents

# Angular guidelines and best practices

## Use the Angular CLI

Angular provides its [own command-line interface](https://angular.dev/tools/cli) which can be used to help you with anything, from scuffolding a new app to generating boilerplate for all primitives and even [migrating your app](#keeping-angular-up-to-date) to a new version of Angular.

Make sure to use the CLI rather than manually creating your files. CLI generation saves you a lot of time, prevents potential mistakes and follows the best practices.

Each command allows certain type of arguments to be passed in cases where you want to override default options.
A full reference for all commands can be found on the [official docs page](https://angular.dev/cli).

### Example for component generation

For example, the following command will generate a brand new component in its own folder with 4 related files - template (HTML), stylesheet (CSS/SCSS/LESS), TypeScript file with the component class and a TypeScript spec file for unit tests.

```shell
ng generate component cool-page

CREATE src/app/cool-page/cool-page.component.html (24 bytes)
CREATE src/app/cool-page/cool-page.component.spec.ts (569 bytes)
CREATE src/app/cool-page/cool-page.component.ts (309 bytes)
CREATE src/app/cool-page/cool-page.component.scss (0 bytes)
```

The file names are derived by the component name you provide and suffixed with `.component`. The component class name is constructed the same way but in camel case - `CoolPageComponent` and decorated accordingly.

![ng new component example](/img/ng_new_component_example.png)

## Keeping Angular up to date

If you want to update Angular dependencies use the Angular CLI schematics `ng update` instead of running `npm update` for its packages.

- Running `ng update @angular/cli` will update packages from the Angular CLI repo.
- Running `ng update @angular/core` will update packages from the Angular core repo (incl. zone.js).
- Running `ng update @angular/material` will update all packages from the Angular Components repo. If you only use the Angular CDK from that repo you can just run `ng update @angular/cdk`.
- Running `ng update @angular-eslint/schematics` will update all packages from the Angular ESLint repo.

The benefit of using `ng update` is not only the handling of batch updates but it also executes update scripts (if any). That's why it's also the command used for migrating from one major version to another.

Speaking of migrating between major versions of Angular, always use [the official Angular Update Guide](https://update.angular.io) page. It offers a form where you select your current version, the target version you want to migrate to and some additional options.
Then a list of actions is generated for you to guide you throughout the process but don't worry, most (if not all) actions are automatically handled by the update scripts.

## Order attributes in HTML templates

To make component bindings clean and easy to read it's a good idea to put them in a logical order. This can be enforced with the Angular ESLint rule [@angular-eslint/template/attributes-order](https://github.com/angular-eslint/angular-eslint/blob/main/packages/eslint-plugin-template/docs/rules/attributes-order.md).

If you don't specify an order option it defaults to the following one:

- Structural directive (e.g. `*ngIf`, `*ngFor`)
- [Template reference](https://angular.dev/guide/templates/reference-variables)
- Attribute bindings (native DOM attributes and regular string attribute bindings)
- Input bindings (bindings to `@Input` properties of components/directtives)
- Two-way bindings (e.g. `[(ngModel)]`)
- Output bindings (bindings to `@Output` event emitters of components/directtives)

Example:

```html
<app-test-component
  *ngIf="someCondition"
  #testComponent
  someAttribute="plainStringValue"
  [someInput]="someInputValue"
  [(ngModel)]="modelBinding"
  (click)="doSomething()"
/>
```

It's recommended to keep the default order configuration because as it's based on the best practices.

## Angular coding style guide

### File structure conventions

Some code examples display a file that has one or more similarly named companion files. For example, hero.component.ts and hero.component.html.

The guideline uses the shortcut hero.component.ts|html|css|spec to represent those various files. Using this shortcut makes this guide's file structures easier to read and more terse.

### Single responsibility

Apply the [single responsibility principle (SRP)](https://angular.dev/style-guide#single-responsibility) to all components, services, and other symbols. This helps make the application cleaner, easier to read and maintain, and more testable.

Do define one thing, such as a service or component, per file. Also consider adding a single class per file, no matter if it is a regular class or a class for an Angular primitive. Consider limiting files to ~400 lines of code.

> It's strongly recommended to [use the Angular CLI](#use-the-angular-cli) for generating any type of files for your project because it already follows these guidelines.

### File and class naming

Angular has specific naming conventions for files and classes, depending on their purpose, for example:

- File names and classes are usually prefixed with specific words depending on the type of the files - components, directives, services, guards, etc.
- Names of classes and files follow different casing: files follow kebab-case naming while class names follow pascal-case naming.

Make sure you read the official [naming guide](https://angular.dev/style-guide#naming) to get familiar with the specifics and **consider using the CLI for generating files** to make sure that naming conventions are followed correctly.

### Selectors and prefixes

Components and directives in Angular have specific naming conventions for their selectors so make sure you get familiar with them:

- [https://angular.dev/style-guide#component-selectors](Component Selectors)
- [https://angular.dev/style-guide#component-custom-prefix](Component Custom Selectors)
- [https://angular.dev/style-guide#directive-selectors](Directive Selectors)
- [https://angular.dev/style-guide#directive-custom-prefix](Directive Custom Selectors)

> Here we still advice you to [use the Angular CLI](#use-the-angular-cli) to generate your components and directives to make sure that all conventions are followed out of the box.

## Overall structural guidelines

Do start small but keep in mind where the application is heading down the road.

Do have a near term view of implementation and a long term vision.

Do put all of the application's code in a folder named src.

Consider creating a folder for a component when it has multiple accompanying files (.ts, .html, .css, and .spec).

- Helps keep the application structure small and easy to maintain in the early stages, while being easy to evolve as the application grows.

- Components often have four files (for example, *.html,*.css, *.ts, and*.spec.ts) and can clutter a folder quickly.

Here is a compliant folder and file structure:

```file
project root
├── src
│ ├── app
│ │ ├── core
│ │ │ └── exception.service.ts|spec.ts
│ │ │ └── user-profile.service.ts|spec.ts
│ │ ├── heroes
│ │ │ ├── hero
│ │ │ │ └── hero.component.ts|html|css|spec.ts
│ │ │ ├── hero-list
│ │ │ │ └── hero-list.component.ts|html|css|spec.ts
│ │ │ ├── shared
│ │ │ │ └── hero-button.component.ts|html|css|spec.ts
│ │ │ │ └── hero.model.ts
│ │ │ │ └── hero.service.ts|spec.ts
│ │ │ └── heroes.component.ts|html|css|spec.ts
│ │ │ └── heroes.routes.ts
│ │ ├── shared
│ │ │ └── init-caps.pipe.ts|spec.ts
│ │ │ └── filter-text.component.ts|spec.ts
│ │ │ └── filter-text.service.ts|spec.ts
│ │ ├── villains
│ │ │ ├── villain
│ │ │ │ └── …
│ │ │ ├── villain-list
│ │ │ │ └── …
│ │ │ ├── shared
│ │ │ │ └── …
│ │ │ └── villains.component.ts|html|css|spec.ts
│ │ │ └── villains.module.ts
│ │ │ └── villains-routing.module.ts
│ │ └── app.component.ts|html|css|spec.ts
│ │ └── app.routes.ts
│ └── main.ts
│ └── index.html
│ └── …
└── node_modules/…
└── …
```

HELPFUL: While components in dedicated folders are widely preferred, another option for small applications is to keep components flat (not in a dedicated folder).

This adds up to four files to the existing folder, but also reduces the folder nesting. Whatever you choose, be consistent.

### Folders-by-feature structure

Do create folders named for the feature area they represent.

- A developer can locate the code and identify what each file represents at a glance. The structure is as flat as it can be and there are no repetitive or redundant names.

- Helps reduce the application from becoming cluttered through organizing the content.

- When there are a lot of files, for example 10+, locating them is easier with a consistent folder structure and more difficult in a flat structure.

For more information, refer [to this folder and file structure example.](https://angular.dev/style-guide)

## Components

Make sure you read [the official style guide for Angular components](https://angular.dev/style-guide#components). It includes some important guides related to structuring, decorating and refactoring.

## Directives

Make sure you read [the official style guide for Angular directives](https://angular.dev/style-guide#directives). It includes some important guides related to structuring, decorating and refactoring.

## Services

Make sure you read [the official style guide for Angular services](https://angular.dev/style-guide#services).

## Lifecycle hooks

Use Lifecycle hooks to tap into important events exposed by Angular.

### Implement lifecycle hook interfaces

Do implement the lifecycle hook interfaces.

- Lifecycle interfaces prescribe typed method signatures. Use those signatures to flag spelling and syntax mistakes.

```ts
/* avoid */
@Component({
  standalone: true,
  selector: 'toh-hero-button',
  template: `<button type="button">OK</button>`
})
export class HeroButtonComponent {
  onInit() { // misspelled
    console.log('The component is initialized');
  }
}
```

```ts
// GOOD
@Component({
  standalone: true,
  selector: 'toh-hero-button',
  template: `<button type="button">OK</button>`
})
export class HeroButtonComponent implements OnInit {
  ngOnInit() {
    console.log('The component is initialized');
  }
}
```

## Use RxJS properly

While learning Angular you eventually find out that it uses RxJS for many of its APIs and the framework embraces RxJS Observables for state management and reactivity. That's why it's important to have a good understanding of RxJS and follow some good practices when using observables in Angular to avoid potential issues.

### Don't forget to track and unsubscribe

If we ignore some specific cases like HTTP requests, observables usually represent a stream of data and they may emit values many times before they complete (or even never complete).
This is a serious paradigme shift for developers who are used to work with Promises and forgetting to unsubscribe from an observable may result in memory leaks and unwanted behavior. Subscriptions can be handled in a few different ways:

- Assign the subscription to a variably/property and manually unsubscribe from it when it's no longer needed.
- Whenever possible, use the [async pipe](https://angular.dev/api/common/AsyncPipe) to bind observables in templates. The async pipe will automatically unsubscribe when the component is destroyed.
- For Angular 16+ apps you can use the [RxJS interop package](https://angular.dev/guide/signals/rxjs-interop) which allows converting obsrvables to [signals](https://angular.dev/guide/signals). Signals will automatically unsubscribe when the component is destroyed.
- Use RxJS operators to control and limit observables. Operators like [take](https://rxjs.dev/api/operators/take) and [takeUntil](https://rxjs.dev/api/operators/takeUntil) can be used to derive observables that will emit and complete at certain conditions.

### Combine observables with the use of operators instead of nesting subscriptions

Subscribing to one Observable and then subscribing to a 2nd Observable inside the subscription of the 1st one (and so on) is considered a bad practice. This makes code difficult to read, but more importantly - makes subscriptions difficult to track and unsubscribe.

```ts
// ❌ Don't
observable1.subscribe((res1) => {
  observable2(res1).subscribe((res2) => {
    foo(res2);
  });
});

// ✅ Do
observable1.pipe(
  switchMap((res1) => observable1(res1))
).subscribe((res2) => foo(res2))
```

## The RxJS Operator Decision Tree

RxJS offers a quite large set of functions and sometimes even experienced developers can get lost and forget what to use in a given situation or run into an entirely new case where a new approach is needed. This is where the [Operator Decision Tree](https://rxjs.dev/operator-decision-tree) comes to help.
This is a page from the official RxJS website, and it comes with a friendly wizard form. The form has 2 or more steps (depending on the path you choose) and on each step you must select the option that best describe your case and at the end you'll be provided with an answer that links you to the exact RxJS function(S) you'll need.

![RxJS Operator Decision Tree Example](/img/rxjs_odt_example.png)

## Optimizations

### Change detection and runtime optimizations

To keep track of changes in the entire app and reflect them on the UI Angular implements a change detection mechanism that relies on the Zone.js library. Since it has its specifics, the Angular team has created a [change detection guide](https://angular.dev/best-practices/runtime-performance) that helps developers to understand how it works and how to optimize it.

- Use OnPush change detection to [skip component sub-trees](https://angular.dev/best-practices/skipping-subtrees) from unnecessary re-evaluation during CD cycles. You can enforce OnPush change detection strategy for components using the Angular ESLint rule [@angular-eslint/prefer-on-push-component-change-detection](https://github.com/angular-eslint/angular-eslint/blob/main/packages/eslint-plugin/docs/rules/prefer-on-push-component-change-detection.md).
- [Run expensive tasks outside the Angular Zone](https://angular.dev/best-practices/zone-pollution#run-tasks-outside-ngzone) to avoid calling change detection in a given piece of code. This is also suitable for initializing 3rd party non-Angular libraries to prevent their event listeners from being patched by zone.js
- Use [signals](https://angular.dev/guide/signals) for properties in template bindings. This is a new feature in Angular that allows fine-grained control over reactivity and change detection.
- Provide a [`trackBy` function to `ngFor` directives](https://angular.dev/guide/directives#tracking-items-with-ngfor-trackby) to optimize item change detection. If you don't provide such function, the full list will be re-rendered on each CD cycle. This can be enforced using the Angular ESLint rule [@angular-eslint/template/use-track-by-function](https://github.com/angular-eslint/angular-eslint/blob/main/packages/eslint-plugin-template/docs/rules/use-track-by-function.md)
- Use virtual scrolling for huge lists of DOM elements to optimize scrolling performance and avoid the page from being blocked. The Angular CDK repo offers the [Scrolling module](https://material.angular.io/cdk/scrolling/overview) which already implements a structural directive for virtual scrolling.
- Avoid calling functions in template bindings. If you bind a function it will be re-executed during each CD cycle, even if it's not necessary. This can be forbidden using the Angular ESLint rule [@angular-eslint/template/no-call-expression](https://github.com/angular-eslint/angular-eslint/blob/main/packages/eslint-plugin-template/docs/rules/no-call-expression.md).
We encourage you to use [pipes](https://angular.dev/guide/pipes) or [computed signals](https://angular.dev/guide/signals#computed-signals) instead.

The following video by the Angular product lead Minko Gechev explains some of these concepts very well and demonstrate the optimization techniques:

[![Watch the video](https://i.ytimg.com/vi/f8sA-i6gkGQ/maxresdefault.jpg)](https://www.youtube.com/watch?v=f8sA-i6gkGQ)

### Bundle size optimizations

As the code base grows larger and the usage of 3rd party libraries increases, so does the size JavaScript bundle after build. There are various optimization techniques that allow tree-shaking unused modules and lazy loading certain modules off the main JS chunk.
This frees up the main JavaScript bundle from code which is not necessary on initial app load.

- [Lazy load your routes](https://angular.dev/guide/ngmodules/lazy-loading#lazy-loading-basics) to ensure the JS modules tied to a given route/feature are only downloaded when they are first needed.
- When route-based lazy loading is not possible (for example - when you want to lazy load a component for a pop-up), use standalone components with dynamic imports and attach them to the view.
- Import modules or standalone classes only where they are needed so the builder can separate them from the main JS chunk. Importing modules directly to the main App module (or App component for standalone apps), will surely bundle them inside the main JS chunk no matter where you use them.

## Use TypeScript modifiers correctly

### Data visibility modifiers

TypeScript supports 3 types of [visibility modifiers for class members](https://www.typescriptlang.org/docs/handbook/2/classes.html#member-visibility) similar to most object-oriented languages. Since we use TypeScript in Angular, it's important to take advantage of this feature and use it accordingly.

| Modifier | Usage in Angular |
|----------|-------|
| public | Use it for members which are supposed to be visible outside the class. For services these could be properties and methods which are supposed to be accessed by the injecting classes. For components these could be properties and methods which are meant to be accessed by a parent component. |
| private | Use it for any members which are only supposed to be visible within the scope of the class. Keep in mind that *private members are not accessible in HTML templates of components.* |
| protected | Protected members are only accessible in inherited classes. In the context of Angular, a good way of using protected modifiers is in component classes for members that you don't want to be visible outside but are used in HTML template bindings. |

In TypeScript all members are public by default if you don't specify a modifier so pay attention to that when you don't expect some members to be public.

### Read-only modifier

In TypeScript you can prevent assignments of members by using the [Read-only modifier](https://www.typescriptlang.org/docs/handbook/2/classes.html#readonly). The `readonly` modifier can also have good implications specific to Angular:

- Event emitters decorated with the `@Output` decorator. They are not supposed to be re-assigned and you can also enforce this with the ESLint rule [@angular-eslint/prefer-output-readonly](https://github.com/angular-eslint/angular-eslint/blob/main/packages/eslint-plugin/docs/rules/prefer-output-readonly.md)
- Dependency injections in the constructor. They are also not supposed to be re-assigned (for unit tests use [appropriate ways of mocking services](https://angular.dev/guide/testing/services#testing-services-with-the-testbed)).
