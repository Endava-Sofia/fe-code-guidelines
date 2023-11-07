# SASS guidelines and best practices

<br />

Sass is a stylesheet language that’s compiled to CSS. It allows you to use variables, nested rules, mixins, functions, and more, all with a fully CSS-compatible syntax. Sass helps keep large stylesheets well-organized and makes it easy to share design within and across projects.

<br/>

## Sass Or SCSS

<br/>

There is quite a lot of confusion regarding the semantics of the name Sass, and for good reason: Sass means both the preprocessor and its own syntax!

You see, Sass initially described a syntax of which the defining characteristic was its indentation-sensitivity. Soon enough, Sass maintainers decided to close the gap between Sass and CSS by providing a CSS-friendly syntax called SCSS for Sassy CSS. The motto is: if it’s valid CSS, it’s valid SCSS.

Since then, Sass (the preprocessor) has been providing two different syntaxes: Sass (not all-caps, please), also known as the indented syntax, and SCSS. Which one to use is pretty much up to you since both are strictly equivalent in features. It’s only a matter of aesthetics at this point.

Sass’ whitespace-sensitive syntax relies on indentation to get rid of braces, semi-colons and other punctuation symbols, leading to a leaner and shorter syntax. Meanwhile, SCSS is easier to learn since it’s mostly some tiny extra bits on top of CSS.

Most of the developers prefer SCSS over Sass because it is closer to CSS and friendlier. Because of that, SCSS is the default syntax throughout these guidelines.

<br/>

## Key Principles

<br/>

Sass should be kept as simple as it can be.

Sass, being intended to write CSS, should not get much more complex than regular CSS. The KISS principle (Keep It Simple Stupid) is key here and may even take precedence over the DRY principle (Don’t Repeat Yourself) in some circumstances.

Sometimes, it’s better to repeat a little to keep the code maintainable, rather than building a top-heavy, unwieldy, unnecessarily complicated system that is completely unmaintainable because it is overly complex.

<br/>

## Syntax & Formatting

<br/>

For the formatting rules, comments, spacing , measuring units etc. you can read in our css style guide. SASS syntax is almost the same as regular css syntax

```scss
  // Good
.foo {
  display: block;
  overflow: hidden;
}

// Bad
.foo {
    display: block; overflow: hidden;

    padding: 0 1em;
}
```

<br/>

### Strings

<br/>

Believe it or not, strings play quite a large role in both CSS and Sass ecosystems. Most CSS values are either lengths or identifiers, so it actually is quite crucial to stick to some guidelines when dealing with strings in Sass.

<br/>

#### ENCODING

To avoid any potential issue with character encoding, it is highly recommended to force UTF-8 encoding in the main stylesheet using the @charset directive. Make sure it is the very first element of the stylesheet and there is no character of any kind before it.

```scss
  @charset 'utf-8';
```

<br/>

#### QUOTES

CSS does not require strings to be quoted, not even those containing spaces. Take font-family names for instance: it doesn’t matter whether you wrap them in quotes for the CSS parser.

Because of this, Sass also does not require strings to be quoted. Even better (and luckily, you’ll concede), a quoted string is strictly equivalent to its unquoted twin (e.g. 'abc' is strictly equal to abc).

That being said, languages that do not require strings to be quoted are definitely a minority and so, strings should always be wrapped with single quotes (') in Sass (single being easier to type than double on qwerty keyboards). Besides consistency with other languages, including CSS’ cousin JavaScript, there are several reasons for this choice:

color names are treated as colors when unquoted, which can lead to serious issues;
most syntax highlighters will choke on unquoted strings;
it helps general readability;
there is no valid reason not to quote strings.

```scss
  // Good
  $direction: 'left';

  // Bad
  $direction: left;
```

<br/>

#### STRINGS AS CSS VALUES

Specific CSS values (identifiers) such as initial or sans-serif require not to be quoted. Indeed, the declaration font-family: 'sans-serif' will silently fail because CSS is expecting an identifier, not a quoted string. Because of this, we do not quote those values.

```scss
// Good
$font-type: sans-serif;

// Bad
$font-type: 'sans-serif';
```

Hence, we can make a distinction between strings intended to be used as CSS values (CSS identifiers) like in the previous example, and strings when sticking to the Sass data type, for instance map keys.

We don’t quote the former, but we do wrap the latter in single quotes.

<br/>

#### STRINGS CONTAINING QUOTES

If a string contains one or several single quotes, one might consider wrapping the string with double quotes (") instead, in order to avoid escaping characters within the string.

```scss
// Good
@warn 'You can\'t do that.';

// Good
@warn "You can't do that.";

// Bad
@warn 'You can't do that.';
```

<br/>

#### URLS

URLs should be quoted as well, for the same reasons as above:

```scss
// Good
.foo {
  background-image: url('/images/kittens.jpg');
}

// Bad
.foo {
  background-image: url(/images/kittens.jpg);
}
```

<br/>

#### UNITS

When dealing with lengths, a 0 value should never ever have a unit.

```scss
// Good
$length: 0;

// Bad
$length: 0em;
```

To add a unit to a number, you have to multiply this number by 1 unit.

```scss
$value: 42;

// Good
$length: $value * 1px;

// Bad
$length: $value + px;
```

<br/>

#### CALCULATIONS

<b>Top-level numeric calculations should always be wrapped in parentheses.</b> Not only does this requirement dramatically improve readability, it also prevents some edge cases by forcing Sass to evaluate the contents of the parentheses.

```scss
// Good
.foo {
  width: (100% / 3);
}

// Bad
.foo {
  width: 100% / 3;
}
```

<br/>

#### COLORS AND VARIABLES

When using a color more than once, store it in a variable with a meaningful name representing the color.

```scss
$sass-pink: hsl(330, 50%, 60%);
```

Now you are free to use this variable wherever you want. However, if your usage is strongly tied to a theme, I would advise against using the variable as is. Instead, store it in another variable with a name explaining how it should be used.

```scss
$main-theme-color: $sass-pink;
```

Doing this would prevent a theme change leading to something like $sass-pink: blue. [This article](https://davidwalsh.name/sass-color-variables-dont-suck) does a good job at explaining why thinking your color variables through is important.

<br/>

## Lists

<br/>

Lists are the Sass equivalent of arrays. A list is a flat data structure (unlike maps) intended to store values of any type (including lists, leading to nested lists).

Lists should respect the following guidelines:

- either inlined or multilines;
- necessarily on multilines if too long to fit on an 80-character line;
- unless used as is for CSS purposes, always comma separated;
- always wrapped in parenthesis;
- trailing comma if multilines, not if inlined.

```scss
// Good
$font-stack: ('Helvetica', 'Arial', sans-serif);

// Good
$font-stack: (
  'Helvetica',
  'Arial',
  sans-serif,
);

// Bad
$font-stack: 'Helvetica' 'Arial' sans-serif;

// Bad
$font-stack: 'Helvetica', 'Arial', sans-serif;

// Bad
$font-stack: ('Helvetica', 'Arial', sans-serif,);
```

<br/>

## Maps

<br/>

With Sass, stylesheet authors can define maps — the Sass term for associative arrays, hashes or even JavaScript objects. A map is a data structure associating keys to values. Both keys and values can be of any data type, including maps although I would not recommend using complex data types as map keys, if only for the sake of sanity.

Maps should be written as follows:

- space after the colon (:);
- opening brace (() on the same line as the colon (:);
- quoted keys if they are strings (which represents 99% of the cases);
- each key/value pair on its own new line;
- comma (,) at the end of each key/value;
- trailing comma (,) on last item to make it easier to add, remove or reorder items;
- closing brace ()) on its own new line;
- no space or new line between closing brace ()) and semi-colon (;).

```scss
// Good
$breakpoints: (
  'small': 767px,
  'medium': 992px,
  'large': 1200px,
);

// Bad
$breakpoints: ( small: 767px, medium: 992px, large: 1200px );
```

<br/>

## CSS Ruleset

<br/>

At this point, this is mostly revising what everybody knows, but here is how a CSS ruleset should be written:

- related selectors on the same line; unrelated selectors on new lines;
- the opening brace ({) spaced from the last selector by a single space;
- each declaration on its own new line;
- a space after the colon (:);
- a trailing semi-colon (;) at the end of all declarations;
- the closing brace (}) on its own new line;
- a new line after the closing brace }.

```scss
// Good
.foo,
.foo-bar,
.baz {
  display: block;
  overflow: hidden;
  margin: 0 auto;
}

// Bad
.foo,
.foo-bar, .baz {
    display: block;
    overflow: hidden;
    margin: 0 auto }
```

Adding to those CSS-related guidelines, we want to pay attention to:

- local variables being declared before any declarations, then spaced from declarations by a new line;
- mixin calls with no @content coming before any declaration;
- nested selectors always coming after a new line;
- mixin calls with @content coming after any nested selector;
- no new line before a closing brace (}).

```scss
.foo,
.foo-bar,
.baz {
  $length: 42em;

  @include ellipsis;
  @include size($length);
  display: block;
  overflow: hidden;
  margin: 0 auto;

  &:hover {
    color: red;
  }

  @include respond-to('small') {
    overflow: visible;
  }
}
```

<br/>

## Selector Nesting

<br/>

One particular feature Sass provides that is being overly misused by many developers is selector nesting. Selector nesting offers a way for stylesheet authors to compute long selectors by nesting shorter selectors within each others.

### GENERAL RULE

For instance, the following Sass nesting:

```scss
.foo {
  .bar {
    &:hover {
      color: red;
    }
  }
}
```

… will generate this CSS:

```css
.foo .bar:hover {
  color: red;
}
```

Along the same lines, since Sass 3.3 it is possible to use the current selector reference (&) to generate advanced selectors. For instance:

```scss
.foo {
  &-bar {
    color: red;
  }
}
```

… will generate this CSS:

```css
.foo-bar {
  color: red;
}
```

This method is often used along with [BEM naming conventions](https://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/) to generate .block__element and .block--modifier selectors based on the original selector (i.e. .block in this case).

The problem with selector nesting is that it ultimately makes code more difficult to read. One has to mentally compute the resulting selector out of the indentation levels; it is not always quite obvious what the CSS will end up being.

This statement becomes truer as selectors get longer and references to the current selector (&) more frequent. At some point, the risk of losing track and not being able to understand what’s going on anymore is so high that it is not worth it.

To prevent such situations,
<b> we advised against nesting more than 3 levels deep.</b>
I would be more drastic and recommend to avoid selector nesting as much as possible.

<br/>

#### EXCEPTIONS

For starters, it is allowed and even recommended to nest pseudo-classes and pseudo-elements within the initial selector.

```scss
.foo {
  color: $red;

  &:hover {
    color: $green;
  }

  &::before {
    content: 'pseudo-element';
  }
}
```

Using selector nesting for pseudo-classes and pseudo-elements not only makes sense (because it deals with closely related selectors), it also helps keep everything about a component at the same place.

Also, when using component-agnostic state classes such as .is-active, it is perfectly fine to nest it under the component’s selector to keep things tidy.

```scss
.foo {
  // …

  &.is-active {
    font-weight: bold;
  }
}
```

<br/>

## Naming Conventions

<br/>

There are a few things you can name in Sass, and it is important to name them well so the whole code base looks both consistent and easy to read:

- variables;
- functions;
- mixins.

Sass placeholders are deliberately omitted from this list since they can be considered as regular CSS selectors, thus following the same naming pattern as classes.

Regarding variables, functions and mixins, we stick to something very CSS-y: <b>lowercase hyphen-delimited</b>, and above all meaningful.

```scss
$vertical-rhythm-baseline: 1.5rem;

@mixin size($width, $height: $width) {
  // …
}

@function opposite-direction($direction) {
  // …
}
```

<br/>

### Constants

As for many languages, I suggest all-caps snakerized variables when they are constants. Not only is this a very old convention, but it also contrasts well with usual lowercased hyphenated variables.

```scss
// Good
$CSS_POSITIONS: (top, right, bottom, left, center);

// Bad
$css-positions: (top, right, bottom, left, center);
```

<br/>

### Namespace

If you intend to distribute your Sass code, in the case of a library, a framework, a grid system or whatever, you might want to consider namespacing all your variables, functions, mixins and placeholders so it does not conflict with anyone else’s code.

For instance, if you work on a Sassy Unicorn project that is meant to be distributed, you could consider using su- as a namespace. It is specific enough to prevent any naming collisions and short enough not to be a pain to write.

```scss
$su-configuration: ( … );

@function su-rainbow($unicorn) {
  // …
}
```

<br/>

## Responsive Web Design And Breakpoints

<br/>

I do not think we still have to introduce Responsive Web Design now that it is everywhere. However you might ask yourself why is there a section about RWD in a Sass styleguide? Actually there are quite a few things that can be done to make working with breakpoints easier, so I thought it would not be such a bad idea to list them here.

<br>

### Naming Breakpoints

I think it is safe to say that media queries should not be tied to specific devices. For instance, this is definitely a bad idea to try targeting iPads or Blackberry phones specifically. Media queries should take care of a range of screen sizes, until the design breaks and the next media query takes over.

For the same reasons, breakpoints should not be named after devices but something more general. Especially since some phones are now bigger than tablets, some tablets bigger than some tiny screen computers, and so on…

```scss
// Good
$breakpoints: (
  'medium': (min-width: 800px),
  'large': (min-width: 1000px),
  'huge': (min-width: 1200px),
);

// Bad
$breakpoints: (
  'tablet': (min-width: 800px),
  'computer': (min-width: 1000px),
  'tv': (min-width: 1200px),
);
```

<br/>

### Breakpoint Manager

Once you have named your breakpoints the way you want, you need a way to use them in actual media queries. There are plenty of ways to do so but I must say I am a big fan of the breakpoint map read by a getter function. This system is both simple and efficient.

```scss
/// Responsive breakpoint manager
/// @access public
/// @param {String} $breakpoint - Breakpoint
/// @requires $breakpoints
@mixin respond-to($breakpoint) {
  $raw-query: map-get($breakpoints, $breakpoint);

  @if $raw-query {
    $query: if(
      type-of($raw-query) == 'string',
      unquote($raw-query),
      inspect($raw-query)
    );

    @media #{$query} {
      @content;
    }
  } @else {
    @error 'No value found for `#{$breakpoint}`. '
      + 'Please make sure it is defined in `$breakpoints` map.';
  }
}
```

<br/>

### Media Queries Usage

Not so long ago, there was quite a hot debate about where media queries should be written: do they belong within selectors (as Sass allows it) or strictly dissociated from them? I have to say I am a fervent defender of the media-queries-within-selectors system, as I think it plays well with the ideas of components.

```scss
.foo {
  color: red;

  @include respond-to('medium') {
    color: blue;
  }
}
```

Leading to the following CSS output:

```css
.foo {
  color: red;
}

@media (min-width: 800px) {
  .foo {
    color: blue;
  }
}
```

<br/>

## Variables

<br/>

Create variables when it makes sense to do so. Do not initiate new variables for the heck of it, it won’t help. A new variable should be created only when all of the following criteria are met:

- the value is repeated at least twice;
- the value is likely to be updated at least once;
- all occurrences of the value are tied to the variable (i.e. not by coincidence).

Basically, there is no point declaring a variable that will never be updated or that is only being used at a single place.

<br/>

### Scoping

Variable scoping in Sass has changed over the years. Until fairly recently, variable declarations within rulesets and other scopes were local by default.

However when there was already a global variable with the same name, the local assignment would change the global variable. Since version 3.4, Sass now properly tackles the concept of scopes and create a new local variable instead.

The docs talk about global variable shadowing. When declaring a variable that already exists on the global scope in an inner scope (selector, function, mixin…), the local variable is said to be shadowing the global one. Basically, it overrides it just for the local scope.

The following code snippet explains the variable shadowing concept.

```scss
// Initialize a global variable at root level.
$variable: 'initial value';

// Create a mixin that overrides that global variable.
@mixin global-variable-overriding {
  $variable: 'mixin value' !global;
}

.local-scope::before {
  // Create a local variable that shadows the global one.
  $variable: 'local value';

  // Include the mixin: it overrides the global variable.
  @include global-variable-overriding;

  // Print the variable’s value.
  // It is the **local** one, since it shadows the global one.
  content: $variable;
}

// Print the variable in another selector that does no shadowing.
// It is the **global** one, as expected.
.other-local-scope::before {
  content: $variable;
}
```

<br/>

### !default Flag

When building a library, a framework, a grid system or any piece of Sass that is intended to be distributed and used by external developers, all configuration variables should be defined with the !default flag so they can be overwritten.

```scss
$baseline: 1em !default;
```

Thanks to this, a developer can define their own $baseline variable before importing your library without seeing their value redefined.

```scss
// Developer’s own variable
$baseline: 2em;

// Your library declaring `$baseline`
@import 'your-library';

// $baseline == 2em;
```

<br/>

### !global Flag

The !global flag should only be used when overriding a global variable from a local scope. When defining a variable at root level, the !global flag should be omitted.

```scss
// Good
$baseline: 2em;

// Bad
$baseline: 2em !global;
```

<br/>

### Multiple Variables Or Maps

There are advantages of using maps rather than multiple distinct variables. The main one is the ability to loop over a map, which is not possible with distinct variables.

Another pro of using a map is the ability to create a little getter function to provide a friendlier API. For instance, consider the following Sass code:

```scss
/// Z-indexes map, gathering all Z layers of the application
/// @access private
/// @type Map
/// @prop {String} key - Layer’s name
/// @prop {Number} value - Z value mapped to the key
$z-indexes: (
  'modal': 5000,
  'dropdown': 4000,
  'default': 1,
  'below': -1,
);

/// Get a z-index value from a layer name
/// @access public
/// @param {String} $layer - Layer’s name
/// @return {Number}
/// @require $z-indexes
@function z($layer) {
  @return map-get($z-indexes, $layer);
}
```

<br/>

## Extend

<br/>

The <code>@extend</code> directive is a powerful feature that is frequently misunderstood. In general, it makes it possible to tell Sass to style a selector A as though it also matched selector B. Needless to say, this can be a valuable ally when writing modular CSS.

However, the true purpose of <code>@extend</code> is to maintain the relationships (constraints) within extended selectors between rulesets.

```scss
%button {
  display: inline-block;
  // … button styles

  // Relationship: a %button that is a child of a %modal
  %modal > & {
    display: block;
  }
}

.button {
  @extend %button;
}

// Yep
.modal {
  @extend %modal;
}

// Nope
.modal {
  @extend %modal;

  > .button {
    @extend %button;
  }
}
```

<br/>

### Extend And Media Queries

You should only extend selectors within the same media scope (@media directive). Think of a media query as another constraint.

```scss
%foo {
  content: 'foo';
}

// Bad
@media print {
  .bar {
    // This doesn't work. Worse: it crashes.
    @extend %foo;
  }
}

// Good
@media print {
  .bar {
    @at-root (without: media) {
      @extend %foo;
    }
  }
}

// Good
%foo {
  content: 'foo';

  &-print {
    @media print {
      content: 'foo print';
    }
  }
}

@media print {
  .bar {
    @extend %foo-print;
  }
}
```

<br/>

## Mixins

<br/>

Link to this chapter: MixinsEdit this chapter on GitHub: Mixins
Mixins are one of the most used features from the whole Sass language. They are the key to reusability and DRY components. And for good reason: mixins allow authors to define styles that can be reused throughout the stylesheet without needing to resort to non-semantic classes such as .float-left.

They can contain full CSS rules and pretty much everything that is allowed anywhere in a Sass document. They can even take arguments, just like functions. Needless to say, the possibilities are endless.

<br/>

### Basics

That being said, mixins are extremely useful and you should be using some. The rule of thumb is that if you happen to spot a group of CSS properties that always appear together for a reason (i.e. not a coincidence), you can put them in a mixin instead.

```scss
/// Helper to clear inner floats
/// @author Nicolas Gallagher
/// @link http://nicolasgallagher.com/micro-clearfix-hack/ Micro Clearfix
@mixin clearfix {
  &::after {
    content: '';
    display: table;
    clear: both;
  }
}
```

Another valid example would be a mixin to size an element, defining both width and height at the same time. Not only would it make the code lighter to type, but also easier to read.

```scss
/// Helper to size an element
/// @author Kitty Giraudel
/// @param {Length} $width
/// @param {Length} $height
@mixin size($width, $height: $width) {
  width: $width;
  height: $height;
}
```

<br/>

### Argument-Less Mixins

Sometimes mixins are used only to avoid repeating the same group of declarations over and over again, yet do not need any parameter or have sensible enough defaults so that we don’t necessarily have to pass arguments.

In such cases, we can safely omit the parentheses when calling them. The @include keyword (or + sign in indented-syntax) already acts as an indicator that the line is a mixin call; there is no need for extra parentheses here.

```scss
// Good
.foo {
  @include center;
}

// Bad
.foo {
  @include center();
}
```

<br/>

### Arguments List

When dealing with an unknown number of arguments in a mixin, always use an arglist rather than a list. Think of arglist as the 8th hidden undocumented data type from Sass that is implicitly used when passing an arbitrary number of arguments to a mixin or a function whose signature contains ....

```scss
@mixin shadows($shadows...) {
  // type-of($shadows) == 'arglist'
  // …
}
```

Now, when building a mixin that accepts a handful of arguments (understand 3 or more), think twice before merging them out as a list or a map thinking it will be easier than passing them all one by one.

Sass is actually pretty clever with mixins and function declarations, so much so that you can actually pass a list or a map as an arglist to a function/mixin so that it gets parsed as a series of arguments.

```scss
@mixin dummy($a, $b, $c) {
  // …
}

// Good
@include dummy(true, 42, 'kittens');

// Bad
$params: (true, 42, 'kittens');
$value: dummy(nth($params, 1), nth($params, 2), nth($params, 3));

// Good
$params: (true, 42, 'kittens');
@include dummy($params...);

// Good
$params: (
  'c': 'kittens',
  'a': true,
  'b': 42,
);
@include dummy($params...);
```

<br/>

### Mixins And Vendor Prefixes

It might be tempting to define custom mixins to handle vendor prefixes for unsupported or partially supported CSS properties

```scss
// Bad
@mixin transform($value) {
  -webkit-transform: $value;
  -moz-transform: $value;
  transform: $value;
}

// Good
/// Mixin helper to output vendor prefixes
/// @access public
/// @author KittyGiraudel
/// @param {String} $property - Unprefixed CSS property
/// @param {*} $value - Raw CSS value
/// @param {List} $prefixes - List of prefixes to output
@mixin prefix($property, $value, $prefixes: ()) {
  @each $prefix in $prefixes {
    -#{$prefix}-#{$property}: $value;
  }

  #{$property}: $value;
}
```

Then using this mixin should be very straightforward:

```scss
.foo {
  @include prefix(transform, rotate(90deg), ('webkit', 'ms'));
}
```

Please keep in mind this is a poor solution. For instance, it cannot deal with complex polyfills such as those required for Flexbox.

<br/>

## Conditional Statements

<br/>

You probably already know that Sass provides conditional statements via the @if and @else directives. Unless you have some medium to complex logic in your code, there is no need for conditional statements in your everyday stylesheets. Actually, they mainly exist for libraries and frameworks.

Anyway, if you ever find yourself in need of them, please respect the following guidelines:

- No parentheses unless they are necessary;
- Always an empty new line before @if;
- Always a line break after the opening brace ({);
- @else statements on the same line as previous closing brace (}).
- Always an empty new line after the last closing brace (}) unless the next line is a closing brace (}).

```scss
// Good
@if $support-legacy {
  // …
} @else {
  // …
}

// Bad
@if ($support-legacy == true) {
  // …
}
@else {
  // …
}
```

When testing for a falsy value, always use the not keyword rather than testing against false or null.

```scss
// Good
@if not index($list, $item) {
  // …
}

// Bad
@if index($list, $item) == null {
  // …
}
```

Always put the variable part on the left side of the statement, and the (un)expected result on the right. Reversed conditional statements often are more difficult to read, especially to unexperienced developers.

```scss
// Good
@if $value == 42 {
  // …
}

// Bad
@if 42 == $value {
  // …
}
```

When using conditional statements within a function to return a different result based on some condition, always make sure the function still has a @return statement outside of any conditional block.

```scss
// Good
@function dummy($condition) {
  @if $condition {
    @return true;
  }

  @return false;
}

// Bad
@function dummy($condition) {
  @if $condition {
    @return true;
  } @else {
    @return false;
  }
}
```

<br/>

## Loops

<br/>

Link to this chapter: LoopsEdit this chapter on GitHub: Loops
Because Sass provides complex data structures such as lists and maps, it is no surprise that it also gives a way for authors to iterate over those entities.

However, the presence of loops usually implies moderately complex logic that probably does not belong to Sass. Before using a loop, make sure it makes sense and that it actually solves an issue.

### Each

The @each loop is definitely the most-used out of the three loops provided by Sass. It provides a clean API to iterate over a list or a map.

```scss
@each $theme in $themes {
  .section-#{$theme} {
    background-color: map-get($colors, $theme);
  }
}
```

When iterating on a map, always use $key and $value as variable names to enforce consistency.

```scss
@each $key, $value in $map {
  .section-#{$key} {
    background-color: $value;
  }
}
```

Also be sure to respect those guidelines to preserve readability:

- Always an empty new line before @each;
- Always an empty new line after the closing brace (}) unless the next line is a closing brace (}).

### For

The @for loop might be useful when combined with CSS’ :nth-* pseudo-classes. Except for these scenarios, prefer an @each loop if you have to iterate over something.

```scss
@for $i from 1 through 10 {
  .foo:nth-of-type(#{$i}) {
    border-color: hsl($i * 36, 50%, 50%);
  }
}
```

Always use $i as a variable name to stick to the usual convention and unless you have a really good reason to, never use the to keyword: always use through. Many developers do not even know Sass offers this variation; using it might lead to confusion.

Also be sure to respect those guidelines to preserve readability:

- Always an empty new line before @for;
- Always an empty new line after the closing brace (}) unless the next line is a closing brace (}).

### While

The @while loop has absolutely no use case in a real Sass project, especially since there is no way to break a loop from the inside. <b>Do not use it.</b>

<br/>

## SassMeister

SassMeister is a [playground for Sass](https://www.sassmeister.com/). Add some Sass and SassMeister will show you the rendered CSS. SassMeister supports both Sass and SCSS syntaxes, all output styles, and an ever-expanding list of Sass libraries.
