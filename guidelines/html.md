# HTML guidelines and best practices

<br/>
A consistent, clean, and tidy HTML code makes it easier for others to read and understand your code.

Here are some guidelines and tips for creating good HTML code.

<br/>

## Always Declare Document Type

<br/>
Always declare the document type as the first line in your document.

The correct document type for HTML is:

```html
<!DOCTYPE html>
```

If you want consistency with lower case tags, you can use:

```html
<!doctype html>
```

<br/>

## Use Lower Case Element Names

<br/>
HTML5 allows mixing uppercase and lowercase letters in element names.

We recommend using lowercase element names:

- Mixing uppercase and lowercase names is bad
- Developers are used to using lowercase names (as in XHTML)
- Lowercase look cleaner
- Lowercase are easier to write

```html
<section>
  <p>This is a paragraph.</p>
</section>
```

<br/>

## Close All HTML Elements

<br/>
In HTML5, you don't have to close all elements (for example the <p> element).

We recommend closing all HTML elements:

```html
<section>
  <p>This is a paragraph.</p>
  <p>This is a paragraph.</p>
</section>
```
Or self-closing tags
```html
<input />
<img />
<hr />
etc.
```
<br/>

## Close Empty HTML Elements

<br/>
In HTML5, it is optional to close empty elements.

This is allowed:

```html
<meta charset="utf-8" />
```

This is also allowed:

```html
<meta charset="utf-8">
```

The slash (/) is required in XHTML and XML.

If you expect XML software to access your page, it might be a good idea to keep it.

<br/>

## Use Lower Case Attribute Names

<br/>
HTML5 allows mixing uppercase and lowercase letters in attribute names.

We recommend using lowercase attribute names:

- Mixing uppercase and lowercase names is bad
- Developers are used to using lowercase names (as in XHTML)
- Lowercase look cleaner
- Lowercase are easier to write

Looking good:

```html
<div class="menu"></div>
```

<br/>

## Always Quote Attribute Values

<br/>
HTML allows attribute values without quotes.

However, we recommend quoting attribute values, because:

- Developers normally quote attribute values
- Quoted values are easier to read
- You MUST use quotes if the value contains spaces

```html
Good:
<table class="striped"></table>
```

```html
Bad:
<table class=striped></table>
```

```html
Very bad: This will not work, because the value contains spaces:

<table class=table striped></table>
```

<br/>

## Always Specify alt, width, and height for Images

<br/>
Always specify the "alt" attribute for images. This attribute is important if the image for some reason cannot be displayed.

Also, always define the width and height of images. This reduces flickering, because the browser can reserve space for the image before loading.

```html
Good: <img src="images/logo.jpg" alt="A circular globe icon" class="img" />
```

```html
Bad: <img src="images/logo.jpg" />
```

<br/>

## Spaces and Equal Signs

<br/>
HTML allows spaces around equal signs. But space-less is easier to read and groups entities better together.

```html
Good: <link rel="stylesheet" href="styles.css" />
```

```html
Bad: <link rel = "stylesheet"   href = " styles.css" />
```

<br/>

## Avoid Long Code Lines

<br/>
When using an HTML editor, it is NOT convenient to scroll right and left to read the HTML code.

Try to avoid too long code lines.

<br/>

## Blank Lines and Indentation

<br/>
Do not add blank lines, spaces, or indentations without a reason.

For readability, add blank lines to separate large or logical code blocks.

For readability, add two spaces of indentation. Do not use the tab key.

```html
Good:
<body>
  <h1>Famous Cities</h1>

  <h2>Tokyo</h2>
  <p>Tokyo is the capital of Japan, the center of the Greater Tokyo Area, and the most populous metropolitan area in the world.</p>

  <h2>London</h2>
  <p>London is the capital city of England. It is the most populous city in the United Kingdom.</p>

  <h2>Paris</h2>
  <p>Paris is the capital of France. The Paris area is one of the largest population centers in Europe.</p>
</body>
```

```html
Bad:
<body>
<h1>Famous Cities</h1>
<h2>Tokyo</h2>
<p>Tokyo is the capital of Japan, the center of the Greater Tokyo Area, and the most populous metropolitan area in the world.</p>
<h2>London</h2>
<p>London is the capital city of England. It is the most populous city in the United Kingdom.</p>
<h2>Paris</h2>
<p>Paris is the capital of France. The Paris area is one of the largest population centers in Europe.</p>
</body>
```

```html
Good Table Example:
<table>
  <tr>
    <th>Name</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>A</td>
    <td>Description of A</td>
  </tr>
  <tr>
    <td>B</td>
    <td>Description of B</td>
  </tr>
</table>
```

```html
Good List Example:
<ul>
  <li>London</li>
  <li>Paris</li>
  <li>Tokyo</li>
</ul>
```

<br/>

## Never Skip the \<title> Element

<br/>

The \<title> element is required in HTML.

The contents of a page title is very important for search engine optimization (SEO)! The page title is used by search engine algorithms to decide the order when listing pages in search results.

The \<title> element:

- defines a title in the browser toolbar
- provides a title for the page when it is added to favorites
- displays a title for the page in search-engine results
  So, try to make the title as accurate and meaningful as possible:

```html
<title>HTML Style Guide and Coding Conventions</title>
```

<br/>

## Omitting \<html> and \<body>?

<br/>
An HTML page will validate without the \<html> and \<body> tags:

```html
Example
<!DOCTYPE html>
<head>
  <title>Page Title</title>
</head>

<h1>This is a heading</h1>
<p>This is a paragraph.</p>
```

However, we strongly recommend to always add the \<html> and \<body> tags!

Omitting \<body> can produce errors in older browsers.

Omitting \<html> and \<body> can also crash DOM and XML software.

<br/>

## Omitting \<head>?

<br/>
The HTML \<head> tag can also be omitted.

Browsers will add all elements before \<body>, to a default \<head> element.

```html
Example
<!DOCTYPE html>
<html>
  <title>Page Title</title>
  <body>
    <h1>This is a heading</h1>
    <p>This is a paragraph.</p>
  </body>
</html>
```

However, we recommend using the \<head> tag.

<br/>

## Close Empty HTML Elements?

<br/>
In HTML, it is optional to close empty elements.

```html
Allowed: <meta charset="utf-8" />
```

```html
Also Allowed: <meta charset="utf-8">
```

If you expect XML/XHTML software to access your page, keep the closing slash (/), because it is required in XML and XHTML.

<br/>

## Add the lang Attribute

<br/>
You should always include the lang attribute inside the <html> tag, to declare the language of the Web page. This is meant to assist search engines and browsers.

```html
Example
<!DOCTYPE html>
<html lang="en-us">
  <head>
    <title>Page Title</title>
  </head>
  <body>
    <h1>This is a heading</h1>
    <p>This is a paragraph.</p>
  </body>
</html>
```

<br/>

## Meta Data

<br/>
To ensure proper interpretation and correct search engine indexing, both the language and the character encoding <meta charset="charset"> should be defined as early as possible in an HTML document:

```html
<!DOCTYPE html>
<html lang="en-us">
  <head>
    <meta charset="UTF-8" />
    <title>Page Title</title>
  </head>
</html>
```

<br/>

## Setting The Viewport

<br/>
The viewport is the user's visible area of a web page. It varies with the device - it will be smaller on a mobile phone than on a computer screen.

You should include the following <meta> element in all your web pages:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

This gives the browser instructions on how to control the page's dimensions and scaling.

The width=device-width part sets the width of the page to follow the screen-width of the device (which will vary depending on the device).

The initial-scale=1.0 part sets the initial zoom level when the page is first loaded by the browser.

Here is an example of a web page without the viewport meta tag, and the same web page with the viewport meta tag:

<br/>

## Boolean attributes

<br/>

Don't include values for boolean attributes (but do include values for enumerated attributes); you can just write the attribute name to set it. For example, you can write:

```html
Good <input required />
```

This is perfectly understandable and works fine. If a boolean HTML attribute is present, the value is true. While including a value will work, it is not necessary and incorrect:

```html
Bad <input required="required" />
```

<br/>

## Entity references

<br/>
Don't use entity references unnecessarily — use the literal character wherever possible (you'll still need to escape characters like angle brackets and quote marks).

As an example, you could just write:

```html
Good
<p>© 2018 Me</p>
```

Instead of:

```html
Bad
<p>&copy; 2018 Me</p>
```

<br/>

## HTML Comments

<br/>

It is advisable to use short comments like-

```html
<!-- following is a paragraph -->
```

Long comments like-

```html
<!--
    This is a long comment example.
    This is a long comment example.
-->
```

Long comments become easily observable when indented with two spaces.

<br/>

## HTML Line-Wrapping

<br/>

```html
<button
  mat-icon-button
  color="primary"
  class="menu-button"
  (click)="openMenu()"
>
  <mat-icon>menu</mat-icon>
</button>
```

<br/>

## Use Lower Case File Names

<br/>
Some web servers (Apache, Unix) are case sensitive about file names: "london.jpg" cannot be accessed as "London.jpg".

Other web servers (Microsoft, IIS) are not case sensitive: "london.jpg" can be accessed as "London.jpg".

If you use a mix of uppercase and lowercase, you have to be aware of this.

If you move from a case-insensitive to a case-sensitive server, even small errors will break your web!

To avoid these problems, always use lowercase file names!

<br/>

## File Extensions

<br/>
HTML files should have a .html extension (.htm is allowed).

CSS files should have a .css extension.

JavaScript files should have a .js extension.

Differences Between .htm and .html?
There is no difference between the .htm and .html file extensions!

Both will be treated as HTML by any web browser and web server.

<br/>

## Default Filenames

<br/>
When a URL does not specify a filename at the end (like "<https://www.w3schools.com/>"), the server just adds a default filename, such as "index.html", "index.htm", "default.html", or "default.htm".

If your server is configured only with "index.html" as the default filename, your file must be named "index.html", and not "default.html".

However, servers can be configured with more than one default filename; usually you can set up as many default filenames as you want.

<br/>

<br/>

# HTML Semantic Elements

<br/>

## What are Semantic Elements?

<br/>

Semantic elements = elements with a meaning.

A semantic element clearly describes its meaning to both the browser and the developer.

Examples of non-semantic elements: \<div> and \<span> - Tells nothing about its content.

Examples of semantic elements: \<form>, \<table>, and \<article> - Clearly defines its content.

<br/>

## Semantic Elements in HTML

<br/>

Many web sites contain HTML code like: \<div id="nav"> \<div class="header"> \<div id="footer"> to indicate navigation, header, and footer.

In HTML there are some semantic elements that can be used to define different parts of a web page:

- \<article> - Defines independent, self-contained content
- \<aside> - Defines content aside from the page content
- \<details> - Defines additional details that the user can view or hide
- \<figcaption> - Defines a caption for a \<figure> element
- \<figure> - Specifies self-contained content, like illustrations, diagrams, photos, code listings, etc.
- \<footer> - Defines a footer for a document or section
- \<header> - Specifies a header for a document or section
- \<main> - Specifies the main content of a document
- \<mark> - Defines marked/highlighted text
- \<nav> - Defines navigation links
- \<section> - Defines a section in a document
- \<summary> - Defines a visible heading for a \<details> element
- \<time> - Defines a date/time
