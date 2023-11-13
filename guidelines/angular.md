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

Apply the [single responsibility principle (SRP)](https://en.wikipedia.org/wiki/Single-responsibility_principle) to all components, services, and other symbols. This helps make the application cleaner, easier to read and maintain, and more testable.

### File limit

Do define one thing, such as a service or component, per file. Consider limiting files to 400 lines of code.

- One component per file makes it far easier to read, maintain, and avoid collisions with teams in source control.
- One component per file avoids hidden bugs that often arise when combining components in a file where they may share variables, create unwanted closures, or unwanted coupling with dependencies.
- A single component can be the default export for its file which facilitates lazy loading with the router.

The key is to make the code more reusable, easier to read, and less mistake-prone

### Naming

Do use consistent names for all symbols.

Do follow a pattern that describes the symbol's feature then its type. The recommended pattern is feature.type.ts.

- Naming conventions help provide a consistent way to find content at a glance. Consistency within the project is vital. Consistency with a team is important. Consistency across a company provides tremendous efficiency.

- The naming conventions should help find desired code faster and make it easier to understand.

- Names of folders and files should clearly convey their intent. For example, app/heroes/hero-list.component.ts may contain a component that manages a list of heroes.

### Separate file names with dots and dashes

Do use dashes to separate words in the descriptive name.

Do use dots to separate the descriptive name from the type.

Do use consistent type names for all components following a pattern that describes the component's feature then its type. A recommended pattern is feature.type.ts.

Do use conventional type names including .service, .component, .pipe, .module, and .directive. Invent additional type names if you must but take care not to create too many.

- Type names provide a consistent way to quickly identify what is in the file.

- Type names make it easy to find a specific file type using an editor or IDE's fuzzy search techniques.

- Unabbreviated type names such as .service are descriptive and unambiguous. Abbreviations such as .srv, .svc, and .serv can be confusing.

- Type names provide pattern matching for any automated tasks.

### Symbols and file names

Do use consistent names for all assets named after what they represent.

Do use upper camel case for class names.

Do match the name of the symbol to the name of the file.

Do append the symbol name with the conventional suffix (such as Component, Directive, Module, Pipe, or Service) for a thing of that type.

Do give the filename the conventional suffix (such as .component.ts, .directive.ts, .module.ts, .pipe.ts, or .service.ts) for a file of that type.

- Consistent conventions make it easy to quickly identify and reference assets of different types

```ts
@Component({ … }) export class AppComponent { } // app.component.ts
@Component({ … }) export class HeroesComponent { } // heroes.component.ts
@Component({ … }) export class HeroListComponent { } // hero-list.component.ts
@Component({ … }) export class HeroDetailComponent { } // hero-detail.component.ts
@Directive({ … }) export class ValidationDirective { } // validation.directive.ts
@NgModule({ … }) export class AppModule // app.module.ts
@Pipe({ name: 'initCaps' }) export class InitCapsPipe implements PipeTransform { } // init-caps.pipe.ts
@Injectable() export class UserProfileService { } // user-profile.service.ts
```

### Service names

Do use consistent names for all services named after their feature.

Do suffix a service class name with Service. For example, something that gets data or heroes should be called a DataService or a HeroService.

A few terms are unambiguously services. They typically indicate agency by ending in "-er". You may prefer to name a service that logs messages Logger rather than LoggerService. Decide if this exception is agreeable in your project. As always, strive for consistency.

- Provides a consistent way to quickly identify and reference services.

- Clear service names such as Logger do not require a suffix.

- Service names such as Credit are nouns and require a suffix and should be named with a suffix when it is not obvious if it is a service or something else.

```ts
@Injectable() export class HeroDataService { } // hero-data.service.ts
@Injectable() export class CreditService { } // credit.service.ts
@Injectable() export class Logger { } // logger.service.ts
```

### Component selectors

Do use dashed-case or kebab-case for naming the element selectors of components.

- Keeps the element names consistent with the [specification for Custom Elements](https://www.w3.org/TR/custom-elements/).

```ts
/* avoid */
@Component({
  standalone: true,
  selector: 'tohHeroButton',
  templateUrl: './hero-button.component.html'
})
export class HeroButtonComponent { }
```

```html
// Good
<toh-hero-button></toh-hero-button>
```

```ts
// Good
import { Component } from '@angular/core';
@Component({
  standalone: true,
  selector: 'toh-hero-button',
  templateUrl: './hero-button.component.html'
})
export class HeroButtonComponent { }
```

### Component custom prefix

Do use a hyphenated, lowercase element selector value; for example, admin-users.

Do use a prefix that identifies the feature area or the application itself.

- Prevents element name collisions with components in other applications and with native HTML elements.

- Makes it easier to promote and share the component in other applications.

- Components are easy to identify in the DOM.

```ts
app/heroes/hero.component.ts


check
/* avoid */
// HeroComponent is in the Tour of Heroes feature
// app/heroes/hero.component.ts
@Component({
  standalone: true,
  selector: 'hero',
  template: '',
})
export class HeroComponent { }
```

```ts
/* avoid */
// UsersComponent is in an Admin feature
// app/users/users.component.ts
@Component({
  standalone: true,
  selector: 'users',
  template: '',
})
export class UsersComponent { }
```

```ts
// Good
// app/heroes/hero.component.ts
@Component({
...
  standalone: true,
  selector: 'toh-hero',
})
export class HeroComponent { }
```

```ts
// Good
// app/users/users.component.ts
@Component({
...
  standalone: true,
  selector: 'admin-users'
})
export class UsersComponent { }
```

### Directive selectors

Do Use lower camel case for naming the selectors of directives.

- Keeps the names of the properties defined in the directives that are bound to the view consistent with the attribute names.

- The Angular HTML parser is case-sensitive and recognizes lower camel case.

### Directive custom prefix

Do spell non-element selectors in lower camel case unless the selector is meant to match a native HTML attribute.

Don't prefix a directive name with ng because that prefix is reserved for Angular and using it could cause bugs that are difficult to diagnose.

- Prevents name collisions.

- Directives are easily identified.

```ts
// app/shared/validate.directive.ts
/* avoid */
@Directive({
  standalone: true,
  selector: '[validate]',
})
export class ValidateDirective { }
```

```ts
// Good
// app/shared/validate.directive.ts
@Directive({
  standalone: true,
  selector: '[tohValidate]',
})
export class ValidateDirective { }
```

### Pipe names

Do use consistent names for all pipes, named after their feature. The pipe class name should use UpperCamelCase (the general convention for class names), and the corresponding name string should use lowerCamelCase. The name string cannot use hyphens ("dash-case" or "kebab-case").

- Provides a consistent way to quickly identify and reference pipes.

```ts
@Pipe({ standalone: true, name: 'ellipsis' }) export class EllipsisPipe implements PipeTransform { } // ellipsis.pipe.ts

@Pipe({ standalone: true, name: 'initCaps' }) export class InitCapsPipe implements PipeTransform { } // init-caps.pipe.ts
```

### Unit test file names

Do name test specification files the same as the component they test.

Do name test specification files with a suffix of .spec.

- Provides a consistent way to quickly identify tests.

- Provides pattern matching for karma or other test runners.

```ts
// Components
heroes.component.spec.ts
hero-list.component.spec.ts
hero-detail.component.spec.ts

// Services
logger.service.spec.ts
hero.service.spec.ts
filter-text.service.spec.ts

// Pipes
ellipsis.pipe.spec.ts
init-caps.pipe.spec.ts
```

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

### Components as elements

Consider giving components an element selector, as opposed to attribute or class selectors.

- Components have templates containing HTML and optional Angular template syntax. They display content. Developers place components on the page as they would native HTML elements and web components.

- It is easier to recognize that a symbol is a component by looking at the template's html.

HELPFUL: There are a few cases where you give a component an attribute, such as when you want to augment a built-in element.

For example, [Material Design](https://material.angular.io/components/button/overview) uses this technique with

```html
<button mat-button>
```

However, you wouldn't use this technique on a custom element.

```ts
// app/heroes/hero-button/hero-button.component.ts
/* avoid */
@Component({
  standalone: true,
  selector: '[tohHeroButton]',
  templateUrl: './hero-button.component.html'
})
export class HeroButtonComponent { }
```

```html
<!-- avoid -->
<div tohHeroButton></div>
```

```html
<!-- GOOD -->
<toh-hero-button></toh-hero-button>
```

```ts
// GOOD
import { Component } from '@angular/core';
@Component({
  standalone: true,
  selector: 'toh-hero-button',
  templateUrl: './hero-button.component.html'
})
export class HeroButtonComponent { }
```

### Extract templates and styles to their own files

Do extract templates and styles into a separate file, when more than 3 lines.

Do name the template file [component-name].component.html, where [component-name] is the component name.

Do name the style file [component-name].component.css, where [component-name] is the component name.

Do specify component-relative URLs, prefixed with ./.

- Large, inline templates and styles obscure the component's purpose and implementation, reducing readability and maintainability.

- In most editors, syntax hints and code snippets aren't available when developing inline templates and styles. The Angular TypeScript Language Service (forthcoming) promises to overcome this deficiency for HTML templates in those editors that support it; it won't help with CSS styles.

- A component relative URL requires no change when you move the component files, as long as the files stay together.

- The ./ prefix is standard syntax for relative URLs; don't depend on Angular's current ability to do without that prefix.

```ts

/* avoid */
@Component({
  standalone: true,
  selector: 'toh-heroes',
  template: `
    <div>
      <h2>My Heroes</h2>
      <ul class="heroes">
        @for (hero of heroes | async; track hero) {
          <li (click)="selectedHero=hero">
            <span class="badge">{{hero.id}}</span> {{hero.name}}
          </li>
        }
      </ul>
      @if (selectedHero) {
        <div>
          <h2>{{selectedHero.name | uppercase}} is my hero</h2>
        </div>
      }
    </div>
  `,
  styles: [`
    .heroes {
      margin: 0 0 2em 0;
      list-style-type: none;
      padding: 0;
      width: 15em;
    }
    .heroes li {
      cursor: pointer;
      position: relative;
      left: 0;
      background-color: #EEE;
      margin: .5em;
      padding: .3em 0;
      height: 1.6em;
      border-radius: 4px;
    }
    .heroes .badge {
      display: inline-block;
      font-size: small;
      color: white;
      padding: 0.8em 0.7em 0 0.7em;
      background-color: #607D8B;
      line-height: 1em;
      position: relative;
      left: -1px;
      top: -4px;
      height: 1.8em;
      margin-right: .8em;
      border-radius: 4px 0 0 4px;
    }
  `],
  imports: [NgFor, NgIf, AsyncPipe, UpperCasePipe]
})
export class HeroesComponent {
  heroes: Observable<Hero[]>;
  selectedHero!: Hero;
  constructor(private heroService: HeroService) {
    this.heroes = this.heroService.getHeroes();
  }
}
```

GOOD

```html
<div>
  <h2>My Heroes</h2>
  <ul class="heroes">
    @for (hero of heroes | async; track hero) {
      <li>
        <button type="button" (click)="selectedHero=hero">
          <span class="badge">{{ hero.id }}</span>
          <span class="name">{{ hero.name }}</span>
        </button>
      </li>
    }
  </ul>
  @if (selectedHero) {
    <div>
      <h2>{{ selectedHero.name | uppercase }} is my hero</h2>
    </div>
  }
</div>
```

```css
.heroes {
  margin: 0 0 2em 0;
  list-style-type: none;
  padding: 0;
  width: 15em;
}
.heroes li {
  display: flex;
}
.heroes button {
  flex: 1;
  cursor: pointer;
  position: relative;
  left: 0;
  background-color: #EEE;
  margin: .5em;
  padding: 0;
  border-radius: 4px;
  display: flex;
  align-items: stretch;
  height: 1.8em;
}
.heroes button:hover {
  color: #2c3a41;
  background-color: #e6e6e6;
  left: .1em;
}
.heroes button:active {
  background-color: #525252;
  color: #fafafa;
}
.heroes button.selected {
  background-color: black;
  color: white;
}
.heroes button.selected:hover {
  background-color: #505050;
  color: white;
}
.heroes button.selected:active {
  background-color: black;
  color: white;
}
.heroes .badge {
  display: inline-block;
  font-size: small;
  color: white;
  padding: 0.8em 0.7em 0 0.7em;
  background-color: #405061;
  line-height: 1em;
  margin-right: .8em;
  border-radius: 4px 0 0 4px;
}
.heroes .name {
  align-self: center;
}
```

```ts
import { Component } from '@angular/core';
import { Observable } from 'rxjs';
import { Hero, HeroService } from './shared';
import { AsyncPipe, NgFor, NgIf, UpperCasePipe } from '@angular/common';
@Component({
  standalone: true,
  selector: 'toh-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css'],
  imports: [NgFor, NgIf, AsyncPipe, UpperCasePipe],
})
export class HeroesComponent {
  heroes: Observable<Hero[]>;
  selectedHero!: Hero;
  constructor(private heroService: HeroService) {
    this.heroes = this.heroService.getHeroes();
  }
}
```

### Decorate input and output properties

Do use the <code>@Input()</code> and <code>@Output()</code> class decorators instead of the inputs and outputs properties of the @Directive and @Component metadata:

Consider placing <code>@Input()</code> or <code>@Output()</code> on the same line as the property it decorates.

- It is easier and more readable to identify which properties in a class are inputs or outputs.

- If you ever need to rename the property or event name associated with <code>@Input()</code> or <code>@Output()</code>, you can modify it in a single place.

- The metadata declaration attached to the directive is shorter and thus more readable.

- Placing the decorator on the same line usually makes for shorter code and still easily identifies the property as an input or output. Put it on the line above when doing so is clearly more readable.

```ts
/* avoid */
@Component({
  standalone: true,
  selector: 'toh-hero-button',
  template: `<button type="button"></button>`,
  inputs: [
    'label'
  ],
  outputs: [
    'heroChange'
  ]
})
export class HeroButtonComponent {
  heroChange = new EventEmitter<any>();
  label: string = "";
}
```

```ts
// Good
@Component({
  standalone: true,
  selector: 'toh-hero-button',
  template: `<button type="button">{{label}}</button>`
})
export class HeroButtonComponent {
  @Output() heroChange = new EventEmitter<any>();
  @Input() label = '';
}
```

### Delegate complex component logic to services

Do limit logic in a component to only that required for the view. All other logic should be delegated to services.

Do move reusable logic to services and keep components simple and focused on their intended purpose.

- Logic may be reused by multiple components when placed within a service and exposed as a function.

- Logic in a service can more easily be isolated in a unit test, while the calling logic in the component can be easily mocked.

- Removes dependencies and hides implementation details from the component.

- Keeps the component slim, trim, and focused.

```ts
/* avoid */
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { catchError, finalize } from 'rxjs/operators';
import { Hero } from '../shared/hero.model';
const heroesUrl = 'http://angular.io';
@Component({
  standalone: true,
  selector: 'toh-hero-list',
  template: `...`
})
export class HeroListComponent implements OnInit {
  heroes: Hero[];
  constructor(private http: HttpClient) {
    this.heroes = [];
  }
  getHeroes() {
    this.heroes = [];
    this.http.get(heroesUrl).pipe(
      catchError(this.catchBadResponse),
      finalize(() => this.hideSpinner())
    ).subscribe((heroes: Hero[]) => this.heroes = heroes);
  }
  ngOnInit() {
    this.getHeroes();
  }
  private catchBadResponse(err: any, source: Observable<any>) {
    // log and handle the exception
    return new Observable();
  }
  private hideSpinner() {
    // hide the spinner
  }
}
```

```ts
// Good
import { Component, OnInit } from '@angular/core';
import { Hero, HeroService } from '../shared';
@Component({
  standalone: true,
  selector: 'toh-hero-list',
  template: `...`
})
export class HeroListComponent implements OnInit {
  heroes: Hero[] = [];
  constructor(private heroService: HeroService) { }
  getHeroes() {
    this.heroes = [];
    this.heroService.getHeroes()
      .subscribe(heroes => this.heroes = heroes);
  }

  ngOnInit() {
    this.getHeroes();
  }
}
```

### Don't prefix output properties

Do name events without the prefix on.

Do name event handler methods with the prefix on followed by the event name.

- This is consistent with built-in events such as button clicks.

- Angular allows for an alternative syntax on-*. If the event itself was prefixed with on this would result in an on-onEvent binding expression.

```ts
// app/heroes/hero.component.ts
/* avoid */
@Component({
  standalone: true,
  selector: 'toh-hero',
  template: `...`
})
export class HeroComponent {
  @Output() onSavedTheDay = new EventEmitter<boolean>();
}
```

```html
<!-- avoid -->
<toh-hero (onSavedTheDay)="onSavedTheDay($event)"></toh-hero>
```

```html
<!-- GOOD -->
<toh-hero (savedTheDay)="onSavedTheDay($event)"></toh-hero>
```

```ts
// GOOD
import { Component, EventEmitter, Output } from '@angular/core';
@Component({
  standalone: true,
  selector: 'toh-hero',
  template: `...`
})
export class HeroComponent {
  @Output() savedTheDay = new EventEmitter<boolean>();
}
```

### Put presentation logic in the component class

Do put presentation logic in the component class, and not in the template.

- Logic will be contained in one place (the component class) instead of being spread in two places.

- Keeping the component's presentation logic in the class instead of the template improves testability, maintainability, and reusability.

```ts
/* avoid */
@Component({
  standalone: true,
  selector: 'toh-hero-list',
  template: `
    <section>
      Our list of heroes:
      @for (hero of heroes; track hero) {
        <toh-hero [hero]="hero"></toh-hero>
      }
      Total powers: {{totalPowers}}<br>
      Average power: {{totalPowers / heroes.length}}
    </section>
  `,
  imports: [NgFor, HeroComponent]
})
export class HeroListComponent {
  heroes: Hero[] = [];
  totalPowers: number = 0;
}
```

```ts
// GOOD
@Component({
  standalone: true,
  selector: 'toh-hero-list',
  template: `
    <section>
      Our list of heroes:
      @for (hero of heroes; track hero) {
        <toh-hero [hero]="hero"></toh-hero>
      }
      Total powers: {{totalPowers}}<br>
      Average power: {{avgPower}}
    </section>
  `,
  imports: [NgFor, HeroComponent],
})
export class HeroListComponent {
  heroes: Hero[];
  totalPowers = 0;
...
  get avgPower() {
    return this.totalPowers / this.heroes.length;
  }
}
```

### Initialize inputs

TypeScript's <code>--strictPropertyInitialization</code> compiler option ensures that a class initializes its properties during construction. When enabled, this option causes the TypeScript compiler to report an error if the class does not set a value to any property that is not explicitly marked as optional.

By design, Angular treats all @Input properties as optional. When possible, you should satisfy <code>--strictPropertyInitialization</code> by providing a default value.

```ts
@Component({
  standalone: true,
  selector: 'toh-hero',
  template: `...`
})
export class HeroComponent {
  @Input() id = 'default_id';
}
```

If the property is hard to construct a default value for, use ? to explicitly mark the property as optional.

```ts
@Component({
  standalone: true,
  selector: 'toh-hero',
  template: `...`
})
export class HeroComponent {
  @Input() id?: string;
  process() {
    if (this.id) {
      // ...
    }
  }
}
```

You may want to have a required <code>@Input</code> field, meaning all your component users are required to pass that attribute. In such cases, use a default value. Just suppressing the TypeScript error with <code>!</code> is insufficient and should be avoided because it will prevent the type checker ensure the input value is provided.

```ts
@Component({
  standalone: true,
  selector: 'toh-hero',
  template: `...`
})
export class HeroComponent {
  // The exclamation mark suppresses errors that a property is
  // not initialized.
  // Ignoring this enforcement can prevent the type checker
  // from finding potential issues.
  @Input() id!: string;
}
```

## Directives

### Use directives to enhance an element

Do use attribute directives when you have presentation logic without a template.

- Attribute directives don't have an associated template.

- An element may have more than one attribute directive applied.

```ts
@Directive({
  standalone: true,
  selector: '[tohHighlight]'
})
export class HighlightDirective {
  @HostListener('mouseover') onMouseEnter() {
    // do highlight work
  }
}
```

```html
<div tohHighlight>Bombasta</div>
```

### HostListener/HostBinding decorators versus host metadata

Consider preferring the @HostListener and @HostBinding to the host property of the @Directive and @Component decorators.

Do be consistent in your choice.

- The property associated with <code>@HostBinding</code> or the method associated with <code>@HostListener</code> can be modified only in a single place —in the directive's class. If you use the host metadata property, you must modify both the property/method declaration in the directive's class and the metadata in the decorator associated with the directive.

```ts
import { Directive, HostBinding, HostListener } from '@angular/core';
@Directive({
  standalone: true,
  selector: '[tohValidator]'
})
export class ValidatorDirective {
  @HostBinding('attr.role') role = 'button';
  @HostListener('mouseenter') onMouseEnter() {
    // do work
  }
}
```

Compare with the less preferred <code>host</code> metadata alternative.

- The <code>host</code> metadata is only one term to remember and doesn't require extra ES imports.

```ts
import { Directive } from '@angular/core';
@Directive({
  standalone: true,
  selector: '[tohValidator2]',
  host: {
    '[attr.role]': 'role',
    '(mouseenter)': 'onMouseEnter()'
  }
})
export class Validator2Directive {
  role = 'button';
  onMouseEnter() {
    // do work
  }
}
```

## Services

### Services are singletons

Do use services as singletons within the same injector. Use them for sharing data and functionality.

- Services are ideal for sharing methods across a feature area or an app.

- Services are ideal for sharing stateful in-memory data.

```ts
export class HeroService {
  constructor(private http: HttpClient) { }
  getHeroes() {
    return this.http.get<Hero[]>('api/heroes');
  }
}
```

### Providing a service

Do provide a service with the application root injector in the <code>@Injectable</code> decorator of the service.

- The Angular injector is hierarchical.

- When you provide the service to a root injector, that instance of the service is shared and available in every class that needs the service. This is ideal when a service is sharing methods or state.

- When you register a service in the @Injectable decorator of the service, optimization tools such as those used by the [Angular CLI's](https://angular.dev/cli) production builds can perform tree shaking and remove services that aren't used by your app.

- This is not ideal when two different components need different instances of a service. In this scenario it would be better to provide the service at the component level that needs the new and separate instance.

```ts
import { Injectable } from '@angular/core';
@Injectable({
  providedIn: 'root',
})
export class Service {
}
```

### Use the @Injectable() class decorator

Do use the <code>@Injectable()</code> class decorator instead of the @Inject parameter decorator when using types as tokens for the dependencies of a service.

- The Angular Dependency Injection (DI) mechanism resolves a service's own dependencies based on the declared types of that service's constructor parameters.

- When a service accepts only dependencies associated with type tokens, the <code>@Injectable()</code> syntax is much less verbose compared to using <code>@Inject()</code> on each individual constructor parameter.

```ts
/* avoid */
export class HeroArena {
  constructor(
      @Inject(HeroService) private heroService: HeroService,
      @Inject(HttpClient) private http: HttpClient) {}
}
```

```ts
// GOOD
@Injectable()
export class HeroArena {
  constructor(
    private heroService: HeroService,
    private http: HttpClient) {}
...
}
```

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
