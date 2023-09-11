# Understanding Angular's Change Detection

## Default change detection with Zone.js

### How it works?

To keep track of changes in the entire app and reflect them on the templates Angular relies on [zone.js](https://www.npmjs.com/package/zone.js). Zone.js patches every possible async task (setTimeout, setInterval, Promises, DOM Events, fetch requests, animation frame requests, etc.) and that's how Angular knows when something in the app has changed, and triggers a full change detection cycle which runs throughout the whole component tree from top to bottom (visualized in the animation below).

![Angular Change Detection Cycle Example](/img/angular-cd-cycle.gif)

Because of this change detection mechanism, the developer doesn't really need to think about making their components reactive because it just works out of the box. Zone.js is also helpful when it comes for unit testing because it allows us to flush async tasks instantly and simulate passage of time (ticks) without having to wait. More on async unit testing can be found [here](https://angular.io/guide/testing-components-scenarios#component-with-async-service).

### Troubleshoot performance issues due to poorly handled change detection

Although change detection in Angular is very fast (in fact ~5x faster than its ancestor AngularJS) thanks to the various optimization techniques being applied on production builds, you can see that this way of tracking changes can potentially turn itself against you if you are not careful. Browsers usually run in 60 frames per second, which makes ~16.666 ms per frame, and when change detection cycles start taking longer than that, frame rate drops, and the UI starts lagging due to the main thread being blocked. The reasons for too long change detection cycles could vary depending on the case:

- Large component tree with too many levels. Just like it was explained above - change detection will run throughout the whole component tree from top to bottom, regardless of the origin of the change.
- Large number of components with property bindings (for example huge tables or lists). During a change detection cycle every component that has input properties will be checked for changes even when there's nothing changed.
- Event handlers being attached to high frequency events (knowingly or unknowingly). Every time a DOM event triggers a CD cycle so you can imagine the overhead produced by high frequency events like scroll, mouse move or resize.
- Running extensive code inside a component even if it doesn't introduce a change to the template.
- Probably less likely but still worth mentioning - watch out for common RxJS mistakes like leaving observables without unsubscribing.

It's highly recommended to get familiar with the Angular DevTools for browsers and learn how [profile the Angular application](https://angular.io/guide/devtools#profile-your-application). Profiling allows monitoring of change detection cycles and helps identifying the origin of an issue with large change detection cycles. If a developer identifies any of the issues mentioned above, there are different ways to solve them depending on the case:

- Set the change detection strategy for one or more components to `OnPush` to skip components or entire sub-trees of components during change detection cycles. Angular has 2 change detection strategies: `Default` which is the default one that uses Zone.js like it was already explained, and `OnPush` which only checks a component for changes if there is a change in any of its `@Input()` properties. The use of `OnPush` change detection can be enforced with [this  Angular ESLint rule](https://github.com/angular-eslint/angular-eslint/blob/main/packages/eslint-plugin/docs/rules/prefer-on-push-component-change-detection.md) This has to be done with caution because if you have properties which are used in template bindings, then after changing those properties from inside the component you need to [trigger change detection manually](https://mokkapps.de/blog/the-last-guide-for-angular-change-detection-you-will-ever-need/#trigger-change-detection-manually).
- Provide `trackBy` functions whenever you use `ngFor` in large lists. Angular will compare the results from the `trackBy` function to decide if a given element in the loop needs to be re-rendered or not. If `trackBy` function is not provided, Angular will re-render the whole list during each change detection cycle. This can be enforced using [this  Angular ESLint rule](https://github.com/angular-eslint/angular-eslint/blob/main/packages/eslint-plugin-template/docs/rules/use-track-by-function.md).
- Whenever makes sense run code outside the Angualr zone to avoid unnecessary change detection during extensive tasks. This can be done by injecting the `NgZone` service and use the method `runOutsideAngular()`. Any code passed to the callback of this method will not trigger change detection. A good idea also would be initializing 3rd party non-Angular libraries outside the Angular zone because in some cases they might create various event listeners (for the internal work of the library) that you don't want to be patched by Zone.js.

For more details on the topic [this article](https://mokkapps.de/blog/the-last-guide-for-angular-change-detection-you-will-ever-need/) is highly recommended to read. Another recommendation is this [short and easy to understand video](https://www.youtube.com/watch?v=f8sA-i6gkGQ) by Minko Gechev (Angular Product Lead) on the topic where he covers the most common performance issues.

## Signals

### How Signals work?

Since v16 the Angular team introduced a new way of doing reactive programming and change detection - [signals](https://angular.io/guide/signals). A signal is a wrapper around a value that can notify interested consumers when that value changes. Signals can contain any value, from simple primitives to complex data structures. A signal's value is always read through a getter function, which allows Angular to track where the signal is used. Signals may be either writable or read-only.

```ts
@Component({
  template: `
  <p>Count is {{ count() }}</p>
  <button (click)="increaseConut()">Increase count</button>
  <button (click)="setCount(3)">Set count to 3</button>
  `
})
class TestComponent {
  public count = signal<number>(0);

  public setCount(newCount: number): void {
    count.set(newCount);
  }

  public increaseCount(): void {
    count.update((value) => value + 1);
  }
}
```

### Computed signals

You can also get a computed value from signals using the `computed` signal primitive:

```ts
class TestComponent {
  public firstName = signal<string>('John');
  public lastName = signal<string>('Doe');

  // Computed signal that constructs a full name. The signal will only be re-computed if there's a change in some of the signals used inside
  public fullName = computed(() => `${this.firstName()} ${this.lastName()}`);
}
```

### Effects

An additional primitive introduced along with the signals is `effect` which is used to create side effects using signals.

```ts
class TestComponent {
  public count = signal<number>(0);

  constructor() {
    effect(() => {
      console.log(`The current count is: ${count()}`);
    });
  }
}
```

For more info on advance use of signals and effects please refer to the [official signals guide](https://angular.io/guide/signals#advanced-topics).