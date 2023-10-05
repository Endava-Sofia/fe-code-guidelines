# CSS guidelines and best practices

<br/>

The following guidelines cover how to write CSS example code following the best practices

<br/>

## Choosing a format

<br/>

Opinions on correct indentation, whitespace, and line lengths have always been controversial. Discussions on these topics are a distraction from creating and maintaining content.

You can use Prettier as a code formatter to keep the code style consistent (and to avoid off-topic discussions). You can consult our configuration file to learn about the current rules, and read the [Prettier documentation](https://prettier.io/docs/en/index.html).

Prettier formats all the code and keeps the style consistent. Nevertheless, there are a few additional rules that you need to follow.

### Spacing

<br/>

- Use soft-tabs with a two space indent.
- Put spaces after colon : in property declarations.
- Put spaces before curly bracket { in rule declarations.
- Put spaces after comma ,.
- Seperate rules by new lines.

```css
.foo {
  color: $white;
}
```

### Formating

<br/>

- Use a semicolon ; after every declaration. Even the last one.

```css
.foo {
  color: $white;
  padding: 0;
}
```

- Don't use camelCase or underscores in class names. Use dashes.

```css
.foo-bar {
  /* Some props */
}
```

- Properties within rule sets should each reside on their own line.
- Use hex color codes #000 instead of using rgb.
- Ideally try to use color variables from color palette instead of hex or rgb.
- Use shorthand properties where possible.
- When grouping selectors, keep individual selectors to a single line.
- Avoid specifying units for zero values, e.g., margin: 0; instead of margin: 0px;.
- Prefer border: 0 over border: none.
- Prefer color: #fff instead of color: white.
- Strip out the zero for decimal number, prefer .5s over 0.5s.
- Try to use single ' quotes instead of double quotes”. In rare situtations where both needed inner quotes should always be single.
- Attributes selector should always be enclosed within quotes (Bad example: [type=submit], Good example: [type='submit']
- Set a limit of 80 characters width at CSS files.
- try to set a max nesting limit of 3 levels.
- Use /**/ for comment blocks instead of //.

<br/>

## CSS comments

<br/>

Use CSS-style comments to comment code that isn't self-documenting. Also note that you should leave a space between the asterisks and the comment.

```css
/* This is a CSS-style comment */
```

Put your comments on separate lines preceding the code they are referring to, like so:

```css
h3 {
  /* Creates a red drop shadow, offset 1px right and down, w/2px blur radius */
  text-shadow: 1px 1px 2px red;
  /* Sets the font-size to double the default document font size */
  font-size: 2rem;
}
```

<br/>

## Class Naming

<br/>

Use meaningful or generic class names.

Instead of presentational or cryptic names, always use class names that reflect the purpose of the element in question, or that are otherwise generic.

Names that are specific and reflect the purpose of the element should be preferred as these are most understandable and the least likely to change.

Generic names are simply a fallback for elements that have no particular or no meaning different from their siblings. They are typically needed as “helpers.”

Using functional or generic names reduces the probability of unnecessary document or template changes.

```css
/* Not recommended: meaningless */
.yee-1901 {}

/* Not recommended: presentational */
.button-green {}
.clear {}
```

```css
/* Recommended: specific */
.gallery {}
.login {}
.video {}

/* Recommended: generic */
.aux {}
.alt {}
```

<br/>

### Class Name Style

<br/>

Use class names that are as short as possible but as long as necessary.

Try to convey what a class is about while being as brief as possible.

Using class names this way contributes to acceptable levels of understandability and code efficiency.

```css
/* Not recommended */
.navigation {}
.atr {}
```

```css
/* Recommended */
.nav {}
.author {}
```

<br/>

### Class Name Delimiters

<br/>

Separate words in class names by a hyphen.

Do not concatenate words and abbreviations in selectors by any characters (including none at all) other than hyphens, in order to improve understanding and scannability.

```css
/* Not recommended: does not separate the words “demo” and “image” */
.demoimage {}

/* Not recommended: uses underscore instead of hyphen */
.error_status {}
```

```css
/* Recommended */
.video-id {}
.ads-sample {}
```

<br/>

### Prefixes

<br/>

Prefix selectors with an application-specific prefix (optional).

In large projects as well as for code that gets embedded in other projects or on external sites use prefixes (as namespaces) for class names. Use short, unique identifiers followed by a dash.

Using namespaces helps preventing naming conflicts and can make maintenance easier, for example in search and replace operations.

```css
.adw-help {} /* AdWords */
.maia-note {} /* Maia */
```

<br/>

### Type Selectors

<br/>

Avoid qualifying class names with type selectors.

Unless necessary (for example with helper classes), do not use element names in conjunction with classes.

Avoiding unnecessary ancestor selectors is useful for performance reasons.

```css
/* Not recommended */
ul.example {}
div.error {}
```

```css
/* Recommended */
.example {}
.error {}
```

<br/>

### ID Selectors

<br/>

Avoid ID selectors.

ID attributes are expected to be unique across an entire page, which is difficult to guarantee when a page contains many components worked on by many different engineers. Class selectors should be preferred in all situations.

```css
/* Not recommended */
#example {}
```

```css
/* Recommended */
.example {}
```

<br/>

### Shorthand Properties

<br/>

Use shorthand properties where possible.

CSS offers a variety of shorthand properties (like font) that should be used whenever possible, even in cases where only one value is explicitly set.

Using shorthand properties is useful for code efficiency and understandability.

```css
/* Not recommended */
border-top-style: none;
font-family: palatino, georgia, serif;
font-size: 100%;
line-height: 1.6;
padding-bottom: 2em;
padding-left: 1em;
padding-right: 1em;
padding-top: 0;
```

```css
/* Recommended */
border-top: 0;
font: 100%/1.6 palatino, georgia, serif;
padding: 0 1em 2em;
```

<br/>

### 0 and Units

<br/>

Omit unit specification after “0” values, unless required.

Do not use units after 0 values unless they are required.

```css
flex: 0px; /* This flex-basis component requires a unit. */
flex: 1 1 0px; /* Not ambiguous without the unit, but needed in IE11. */
margin: 0;
padding: 0;
```

<br/>

### Leading 0s

<br/>

Always include leading “0”s in values.

Put 0s in front of values or lengths between -1 and 1.

```css
font-size: 0.8em;
```

<br/>

### Hexadecimal Notation

<br/>

Use 3 character hexadecimal notation where possible.

For color values that permit it, 3 character hexadecimal notation is shorter and more succinct.

```css
/* Not recommended */
color: #eebbcc;
```

```css
/* Recommended */
color: #ebc;
```

<br/>

### Important Declarations

<br/>

Avoid using !important declarations.

These declarations break the natural cascade of CSS and make it difficult to reason about and compose styles. Use selector specificity to override properties instead.

```css
/* Not recommended */
.example {
  font-weight: bold !important;
}
```

```css
/* Recommended */
.example {
  font-weight: bold;
}
```

<br/>

## CSS Formatting Rules

<br/>

### Declaration Order

<br/>

Alphabetize declarations (optional).

Sort declarations consistently within a project. In the absence of tooling to automate and enforce a consistent sort order, consider putting declarations in alphabetical order in order to achieve consistent code in a way that is easy to learn, remember, and manually maintain.

Ignore vendor-specific prefixes for sorting purposes. However, multiple vendor-specific prefixes for a certain CSS property should be kept sorted (e.g. -moz prefix comes before -webkit).

```css
.header {
  background: fuchsia;
  border: 1px solid;
  -moz-border-radius: 4px;
  -webkit-border-radius: 4px;
  border-radius: 4px;
  color: black;
  text-align: center;
  text-indent: 2em;
}
```

<br/>

### Block Content Indentation

<br/>

Indent all block content.

Indent all block content, that is rules within rules as well as declarations, so to reflect hierarchy and improve understanding.

```css
@media screen, projection {

  .footer {
    background: #fff;
    color: #444;
  }

  .test {
    padding: 0;
    margin: 1rem;
  }
}
```

<br/>

### Declaration Stops

<br/>

Use a semicolon after every declaration.

End every declaration with a semicolon for consistency and extensibility reasons.

```css
.test {
  display: block;
  height: 100px;
}
```

<br/>

### Property Name Stops

<br/>

Use a space after a property name’s colon.

Always use a single space between property and value (but no space between property and colon) for consistency reasons.

```css
h3 {
  font-weight: bold;
}
```

<br/>

### Declaration Block Separation

Use a space between the last selector and the declaration block.

Always use a single space between the last selector and the opening brace that begins the declaration block.

The opening brace should be on the same line as the last selector in a given rule.

```css
/* Not recommended: missing space */
.video{
  margin-top: 1em;
}

/* Not recommended: unnecessary line break */
.video
{
  margin-top: 1em;
}
```

```css
/* Recommended */
.video {
  margin-top: 1em;
}
```

<br/>

### Selector and Declaration Separation

<br/>

Separate selectors and declarations by new lines.

Always start a new line for each selector and declaration.

```css
/* Not recommended */
a:focus, a:active {
  position: relative; top: 1px;
}
```

```css
/* Recommended */
h1,
h2,
h3 {
  font-weight: normal;
  line-height: 1.2;
}
```

<br/>

### Rule Separation

<br/>

Separate rules by new lines.

Always put a blank line (two line breaks) between rules.

```css
html {
  background: #fff;
}

body {
  margin: auto;
  width: 50%;
}
```

<br/>

## Mobile-first media queries

<br/>

In a stylesheet that contains media query styles for different target viewport sizes, first include the narrow screen/mobile styling before any other media queries are encountered. Add styling for wider viewport sizes via successive media queries. Following this rule has many advantages that are explained in the Mobile First article.

```css
/* Default CSS layout for narrow screens */

@media (min-width: 480px) {
  /* CSS for medium width screens */
}

@media (min-width: 800px) {
  /* CSS for wide screens */
}

@media (min-width: 1100px) {
  /* CSS for really wide screens */
}
```

<br/>

## Specificity

<br/>

Specificity is the algorithm used by browsers to determine the CSS declaration that is the most relevant to an element, which in turn, determines the property value to apply to the element. The specificity algorithm calculates the weight of a CSS selector to determine which rule from competing CSS declarations gets applied to an element.

<br/>

### How is specificity calculated?

<br/>

Specificity is an algorithm that calculates the weight that is applied to a given CSS declaration. The weight is determined by the number of selectors of each weight category in the selector matching the element (or pseudo-element). If there are two or more declarations providing different property values for the same element, the declaration value in the style block having the matching selector with the greatest algorithmic weight gets applied.

The specificity algorithm is basically a three-column value of three categories or weights - ID, CLASS, and TYPE - corresponding to the three types of selectors. The value represents the count of selector components in each weight category and is written as ID - CLASS - TYPE. The three columns are created by counting the number of selector components for each selector weight category in the selectors that match the element.

<br/>

### Selector weight categories

<br/>

The selector weight categories are listed here in the order of decreasing specificity:

ID column
Includes only ID selectors, such as #example. For each ID in a matching selector, add 1-0-0 to the weight value.

CLASS column
Includes class selectors, such as .myClass, attribute selectors like [type="radio"] and [lang|="fr"], and pseudo-classes, such as :hover, :nth-of-type(3n), and :required. For each class, attribute selector, or pseudo-class in a matching selector, add 0-1-0 to the weight value.

TYPE column
Includes type selectors, such as p, h1, and td, and pseudo-elements like ::before, ::placeholder, and all other selectors with double-colon notation. For each type or pseudo-element in a matching selector, add 0-0-1 to the weight value.

No value
The universal selector (*) and the pseudo-class :where() and its parameters aren't counted when calculating the weight so their value is 0-0-0, but they do match elements. These selectors do not impact the specificity weight value.

Combinators, such as +, >, ~, " ", and ||, may make a selector more specific in what is selected but they don't add any value to the specificity weight.

The & nesting combinator doesn't add specificity weight, but nested rules do. In terms of specificity, and functionality, nesting is very similar to the :is() pseudo-class.

Like nesting, the :is(), :has(), and negation (:not()) pseudo-classes themselves add no weight. The parameters in these selectors, however, do. The specificity weight of each comes from the selector parameter in the list of selectors with the highest specificity. Similarly, with nested selectors, the specificity weight added by the nested selector component is the selector in the comma-separated list of nested selectors with the highest specificity.
[Read more](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity#the_is_not_has_and_css_nesting_exceptions)

You can use css specificity calculator [here](https://polypane.app/css-specificity-calculator/#selector=)

The table below shows some examples on how to calculate specificity values:

| **Selector**           | **Specificity Value** | Calculation                           |
| ---------------------- |:---------------------:| -------------------------------------:|
| p                      | 1                     | 1                                     |
| p.test                 | 11                    | 1 + 10                                |
| p#demo                 | 101                   | 1 + 100                               |
| p style="color: pink;" | 1000                  | 1000                                  |
| #demo                  | 100                   | 100                                   |
| .test                  | 10                    | 10                                    |
| p.test1.test2          | 21                    | 1 + 10 + 10                           |
| #navbar p#demo         |201                    | 100 + 1 + 100                         |
| *                      | 0                     | 0 (the universal selector is ignored) |
