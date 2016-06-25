# Code Guide

> Every line of code should appear to be written by a single person, no matter the number of contributors.[*](http://mdo.github.io/code-guide/)

# TL;DR

* **High-Level**
    * Use meaningful [naming conventions](#naming-conventions); use structural or purposeful names over presentational
    * We use [SCSS](#css), and our own twist on the [SUIT CSS framework](https://github.com/suitcss/suit/tree/master/doc);
    * We promote the use of [OOCSS](#principles) and the [single responsibility principle](#principles)
* **The Details**
    * We use [`js-`](#javascript) prefixed class names for elements being relied upon for javascript selectors
    * We use [`.u-`](#utilities) prefixed class names for single purpose utility classes
    * Our [components](#components) use **meaningful hypens** and **PascalCase**  and follow the [`<ComponentName>[--modifierName|-descendantName]`](#componentname) pattern
    * [`.is-`](#componentnameis-stateofcomponent) prefixed classes for stateful classes (often toggled by js) like `.is-disabled`
    * `$variable` names should follow the [`<property>-<value>`](#variables) pattern
    * Use [`color`](#colors), [`font-size`](#font-size), [`line-height`](#line-height), and [`letter-spacing`](#letter-spacing) variables defined in the `variables.scss` file
    * Mixins should be prefixed with [`.m-`](#mixins)
* **Best Practices**
    * Limit the use of [shorthand declarations](#shorthand-notation)
    * Use white space to improve [legibility](#single-declarations)
    * Avoid [`#ids`](#avoid-ids), [`!important`](#avoid-important), [child selectors](#avoid-child-selector), and [magic numbers](#avoid-magic-numbers);

# HTML

## Naming Conventions [*](https://github.com/necolas/idiomatic-html)

> Use meaningful names; use structural or purposeful names over presentational.

Use clear, thoughtful, and appropriate names for HTML classes. The names should be informative both within HTML and CSS files.

### Practicality over purity

Strive to maintain HTML standards and semantics, but not at the expense of practicality. Use the least amount of markup with the fewest intricacies whenever possible.

Avoid systematic use of abbreviated class names. Don't make things difficult to understand.

```html
<!-- Example with bad names -->
<div class="cb s-scr"></div>

<!-- Example with better names -->
<div class="column-body is-scrollable"></div>
```

# CSS

#### I **highly recommend** you read [this list](https://github.com/itsjustmath/scalable-css-reading-list) - it is the go-to resource for resources related to writing scalable, object-oriented CSS.

## Our Codebase

We strive to maintain a component-based frontend system. It allows for the implementation and composition of loosely coupled, independent units into well-defined composite objects. Components are encapsulated but are able to interoperate via interfaces/events.[*](https://github.com/suitcss/suit/blob/master/doc/design-principles.md)

Our naming conventions are a [twist](https://medium.com/@fat/mediums-css-is-actually-pretty-fucking-good-b8e2a6c78b06) on the [SUIT CSS framework](https://github.com/suitcss/suit/tree/master/doc). Which is to say...
> **They rely on structured class names and meaningful hyphens** (i.e., not using hyphens merely to separate words). This is to help communicate the relationships between classes.


## Principles

### [Object-orientation](http://cssguidelin.es/#object-orientation)
Object-orientation is a programming paradigm that breaks larger programs up into smaller, in(ter)dependent objects that all have their own roles and responsibilities.

When applied to CSS, we call it object-oriented CSS, or OOCSS (coined by Nicole Sullivan). OOCSS deals with the separation of UIs into structure and skin: breaking UI components into their underlying structural forms, and layering their cosmetic forms on separately. This means that we can recycle common and recurring design patterns very cheaply without having to necessarily recycle their specific implementation details at the same time. OOCSS promotes reuse of code, which makes us quicker, as well as keeping the size of our codebase down.

### The Single Responsibility Principle

The single responsibility principle is a paradigm that, very loosely, states that all pieces of code (in our case, classes) should focus on doing one thing and one thing only. More formally:

> …the single responsibility principle states that every context (class, function, variable, etc.) should have a single responsibility, and that responsibility should be entirely encapsulated by the context.

What this means for us is that our CSS should be composed of a series of much smaller classes that focus on providing very specific and limited functionality. This means that we need to decompose UIs into their smallest component pieces that each serve a single responsibility; they all do just one job, but can be very easily combined and composed to make much more versatile and complex constructs.

***

## Javascript

syntax: `js-<targetName>`

JavaScript-specific classes reduce the risk that changing the structure or theme of components will inadvertently affect any required JavaScript behaviour and complex functionality. It is not neccesarry to use them in every case, just think of them as a tool in your utility belt. If you are creating a class, which you dont intend to use for styling, but instead only as a selector in JavaScript, you should probably be adding the js- prefix. In practice this looks like this:

```html
<a href="/login" class="btn btn-primary js-login"></a>
```

**Again, JavaScript-specific classes should not, under any circumstances, be styled.**

## Utilities

Utility classes are **low-level structural and positional traits**. Utilities can be applied directly to any element; multiple utilities can be used together; and utilities can be used alongside component classes.

Utilities exist because **certain CSS properties and patterns are used frequently**. For example: floats, containing floats, vertical alignment, text truncation. Relying on utilities can help to reduce repetition and provide consistent implementations. They also act as a philosophical alternative to functional (i.e. non-polyfill) mixins.

```html
<div class="u-clearfix">
  <p class="u-textTruncate">{$text}</p>
  <img class="u-pullLeft" src="{$src}" alt="">
  <img class="u-pullLeft" src="{$src}" alt="">
  <img class="u-pullLeft" src="{$src}" alt="">
</div>
```

> _Side note_: Try not to go crazy with creation of utilities. A good rule of thumb is to only create utils if you've used that particular solution at least 3 times in your code. For the most part, you won't want to create utilities for things with values that vary (eg. padding, margin).

### `u-utilityName`

Syntax: `u-<utilityName>`

Utilities must use a camel case name, prefixed with a u namespace. What follows is an example of how various utilities can be used to create a simple structure within a component.

```html
<div class="u-clearfix">
  <a class="u-pullLeft" href="{$url}">
    <img class="u-block" src="{$src}" alt="">
  </a>
  <p class="u-sizeFill u-textBreak">
    …
  </p>
</div>
```

## Components

Syntax: `<componentName>[--modifierName|-descendantName]`

Component driven development offers several benefits when reading and writing HTML and CSS:

   * It helps to distinguish between the classes for the root of the component, descendant elements, and modifications.
   * It keeps the specificity of selectors low.
   * It helps to decouple presentation semantics from document semantics.

You can think of components as **custom elements that enclose specific semantics, styling, and behavior**.

### `ComponentName`
The component's name must be written in pascal case.

```
.MyComponent { /* … */ }

<article class=“MyComponent">
  …
</article>
```

### `ComponentName--modifierName`
A component modifier is a class that *modifies the presentation of the base component in some form*. Modifier names must be written in camel case and be separated from the component name by two hyphens. **The class should be included in the HTML in addition to the base component class**.

```
/* Core button */
.Button { /* … */ }

/* Default button style */
.Button--default { /* … */ }

<button class=“Button Button--primary">…</button>
```

### `ComponentName-descendantName`
A component descendant is a class that is attached to a descendant node of a component. It's responsible for applying presentation directly to the descendant on behalf of a particular component. Descendant names must be written in camel case.

```
<article class=“Tweet">
  <header class=“Tweet-header">
    <img class=“Tweet-avatar" src="{$src}" alt="{$alt}">
    …
  </header>
  <div class=“Tweet-body">
    …
  </div>
</article>
```

### `ComponentName.is-stateOfComponent`
Use `is-stateName` for *state-based modifications of components*. The state name must be Camel case. Never style these classes directly; they should always be used as an adjoining class.

JS can add/remove these classes. This means that the same state names can be used in multiple contexts, but **every component must define its own styles for the state** (as they are scoped to the component).

```
.Tweet { /* … */ }
.Tweet.is-expanded { /* … */ }

<article class=“Tweet is-expanded">
  …
</article>
```

## Variables

Syntax: `<property>-<value>`

Variable names in our CSS are structured to provide strong associations between property and use.

The following variable definition is a `color` property, with the value `greenCaribbean`.

```
$color-greenCaribbean: #00BC9B;
```

### Colors
When implementing feature styles, you should only be using color variables provided by colors.scss. When adding color variables use hex instead of RGB (for the sake of brevity). Use [Name that Color](http://chir.ag/projects/name-that-color/) if you would like semantic names for colors

```scss
// right
color: #000;

// wrong
color: rgba(255,255,255,.3);
```


### Font Styles

Refer to `variables.scss` for type size, letter-spacing, and line height. Raw sizes, spaces, and line heights should be avoided outside of `variables.scss`.

#### Font Size

```scss
$fontSize-micro
$fontSize-smallest
$fontSize-smaller
$fontSize-small
$fontSize-base
$fontSize-large
$fontSize-larger
$fontSize-largest
$fontSize-jumbo
```

#### Line Height
`variables.scss` also provides a line height scale. This should be used for blocks of text.

```
$lineHeight-tightest
$lineHeight-tighter
$lineHeight-tight
$lineHeight-baseSans
$lineHeight-base
$lineHeight-loose
$lineHeight-looser
```

#### Letter spacing
Letter spacing should also be controlled with the following var scale.

```scss
$letterSpacing-tightest
$letterSpacing-tighter
$letterSpacing-tight
$letterSpacing-normal
$letterSpacing-loose
$letterSpacing-looser
```

### Mixins
mixin syntax: `m-<propertyName>`

```scss
@mixin m-centerAlign($max-width) {
  display: block;
  max-width: $max-width;
  margin-left: auto;
  margin-right: auto;
}
```

## Best Practices

### Single declarations
In instances where a rule set includes only one declaration, consider removing line breaks for readability and faster editing.

```scss
/* Single declarations on one line */
.span1 { width: 60px; }
.span2 { width: 140px; }
.span3 { width: 220px; }

/* Multiple declarations, one per line */
.sprite {
  display: inline-block;
  width: 16px;
  height: 15px;
  background-image: url(../img/sprite.png);
}
```

### Shorthand notation
Strive to limit use of shorthand declarations to instances where you must explicitly set all the available values.

Common overused shorthand properties include:

```
padding
margin
font
background
border
border-radius
```

Often times we don't need to set all the values a shorthand property represents.

```scss
/* Bad example */
.element {
  margin: 0 0 10px;
  background: red;
  background: url("image.jpg");
  border-radius: 3px 3px 0 0;
}

/* Good example */
.element {
  margin-bottom: 10px;
  background-color: red;
  background-image: url("image.jpg");
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
}
```

## Stuff to Avoid

### [Avoid ID’s](http://oli.jp/2011/ids/)
ID’s are really hard. They are to specific and mostly necessary for JS-Hooks. The better way is to use classes.

### Avoid `!important`
If you need this, than goes something wrong at your CSS-architecture, because you need this only if you want to overwrite styles. If your structure is clean, than you will don’t need this. It’s like to shot with a cannon to sparrows.

### Avoid child-selector
Why I avoid child-selector? It is yet a good CSS-Feature? No I don’t think so. At my start I used it often, but more and more I learned that it makes the module to specific. It depends on the structure of your html, but what is if the order will be changed? **Use classes!**

```scss
// Bad example
// Child-selector
.m-tabs > li {}

// Good example
// Classname
.m-tabs .m-tabs__trigger {}

// Better example
// Less nesting
.m-tabs__trigger {}
```

### Avoid *Magic Numbers*

"Magic number" is an old school programming term for *unnamed numerical constant*. Basically, it's just a random number that happens to just work™ yet is not tied to any logical explanation.

Needless to say magic numbers are a plague and should be avoided at all costs. When you cannot manage to find a reasonable explanation for why a number works, add an extensive comment explaining how you got there and why you think it works. Admitting you don't know why something works is still more helpful to the next developer than them having to figure out what's going on from scratch.

```scss
/* This value is the lowest I could find to align the top of
 `.foo` with its parent. Ideally, we should fix it properly. */
.foo {
  top: 0.327em; /* 1 */
}
```
