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
