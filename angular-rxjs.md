# Proper use of RxJS Observables

While learning Angular you eventually find out that it uses RxJS for many of its APIs and the framework embraces RxJS Observables for state management and reactivity. That's why it's important to have a good understanding of RxJS and knowing what operators to use in different cases.

## Common pitfalls

RxJS provides great power but if not used properly it can also go against you. There are some common pitfalls that beginners can fall into:

- Nesting one observable subscription in one another instead of combining them into a single one using an appropriate RxJS function.
- Not unsubscribing from observables when no longer needed which can cause memory leaks, unwanted callbacks or other unwanted behavior.
- Not limiting or filtering unwanted values emitted from observables
- Using wrong operators
- Others...

## Handling Observable subscriptions

It was already mentioned that one of the [common pitfalls](#common-pitfalls) is leaving observables unsubscribed, which can cause various of unwanted behavior, including memory leaks. Developers who are not familiar with RxJS may be unaware that some observables can emit many times before they complete (or even never complete). This is an important difference between Promises and Observables and needs to be taken into account. There are various ways of unsubscribing from an observable which is no longer needed or transforming it into an observable that emits only when and while you need it.

### Manually unsubscribe

The most straightforward way of tracking a subscription and unsubscribing from it is by assigning it to a variable and manually calling the `.unsubscribe()` method when it makes sense, for example when component is destroyed.

```ts
import { interval } from 'rxjs';

@Component({ ... })
class TestComponent implements OnDestroy {
  private sub?: Subscription;

  public ngOnDestroy(): void {
    // Unsubscribe on component destroy
    this.sub?.unsubscribe();
  }

  public foo(): void {
    // Unsubscribe if already subscribed
    this.sub?.unsubscribe();
    // Create a new subscription
    this.sub = interval(1000).subscribe(() => { /* ... */ });
  }
}
```

### Use the async pipe in template bindings

The main downside of manual handling is that it still relies on the developer and opens the possibility for oversights leaving an observable unsubscribed. It's always good to rely on approaches that eliminate the human factor as much as possible and unsubscribe automatically. One of these approaches is the `async` pipe which is part of the Angular common module. The pipe can be applied on observables or promises in template bindings. When applied to an Observable it subscribes to it so the emitted values can be used in the template, but also automatically unsubscribes upon component destruction so you don't have to worry about it.

```ts
@Component({
  template: `
    <select>
      <option
        *ngFor="let item of (items$ | async)"
        [value]="item.value"
      >{{ item.label }}</option>
    </select>
  `,
})
class TestComponent {
  items$: Observable<Item> = fetchItems();
}
```

While this approach is very convenient and works amazingly, the obvious downside is that it's only for observables in template bindings. For other cases you may resort to some of the next approaches.

### Use RxJS with signals

The relatively new *Signals* feature of Angular allows creating signals from observables (also converting signals to observables) using the RxJS Interop package. Before you go for this approach, please make sure you get familiar with [Signals in Angular](./angular-change-detection.md#signals). Creating a signal that tracks the values of an Observable can be done with the function `toSignal()` from the RxJS interop package. It instantly subscribes to the observable and automatically unsubscribes from it upon destruction of the component (very similar to the [async pipe](#use-the-async-pipe-in-template-bindings)) Check out [the official guide](https://angular.io/guide/rxjs-interop) to learn different ways to use it.

### Use RxJS operators to limit observables

It's also possible to control when a source observable emits and when it completes with the use of special RxJS functions called operators. The operator functions are passed as arguments to the `.pipe()` method of the Obesrvable and each operator outputs a new Observable derived from the sourced one. When an observable moves into a *complete* state, no new values will be emitted. Here are a few examples:

- [takeUntil(notifier: Observable)](https://rxjs.dev/api/operators/takeUntil) - Emits the values emitted by the source Observable until a `notifier` Observable emits a value.
- [take(count: number)](https://rxjs.dev/api/operators/take) - Emits only the first `count` values emitted by the source Observable. Using `take(1)` will emit only once and will instantly complete which is great for cases where we only want the current value of an observable.
- [takeWhile(predicate)](https://rxjs.dev/api/operators/takeWhile) - Emits values emitted by the source Observable so long as each value satisfies the given `predicate` function, and then completes as soon as this predicate is not satisfied.
- [first(predicate?)](https://rxjs.dev/api/operators/first) - Emits only the first value (or the first value that meets the condition of the `predicate`) emitted by the source Observable. If the `predicate` argument is omitted, the operator works just like `take(1)`

## The RxJS Operator Decision Tree

Thanks to its extensive documentation and examples with the famous marble diagrams, RxJS makes it easy to visualize and understand how different functions work. But sometimes even experienced developers can easily get lost and forget what to use in a given case, or even run into an entirely new case where a new approach is needed. This is where the [Operator Decision Tree](https://rxjs.dev/operator-decision-tree) comes to help. This is a page from the official RxJS website, and it comes with a friendly wizard form. The form has 2 or more steps (depending on the path you choose) and on each step you must select the option that best describe your case and at the end you'll be provided with an answer that links you to the exact RxJS function(S) you'll need.

![RxJS Operator Decision Tree Example](/img/rxjs_odt_example.png)

## Converting promises to observables

If you are in a situation where you have mixed use of a Promise and Observable you might want to try converting the promise to an Observable so you can combine it with other observables using any RxJS operators. For this purpose, you can use the [from](https://rxjs.dev/api/index/function/from) function. It creates an `Observable` from an array/array-like object, an iterable object or a Promise. If you pass a `Promsie` object, it will return an `Observable` that emits once and completes when the Promise resolves, or throws an error when the Promsie rejects (passing the same rejection error).

```ts
import { from } from 'rxjs';

let promiseObj: Promise<SomeType>;
...
const promiseAsObservable: Observable<SomeType> = from(promiseObj);
promiseAsObservable.pipe(
  combineLatestWith(() => { ... }) // chain any rxjs operators
).subscribe([result, otherResult]) => { ... });
```