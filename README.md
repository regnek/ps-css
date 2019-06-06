# PS CSS & Sass Style Guide

*How to formulate your styles for projects at Primitive Social*

## Table of Contents

1. [CSS](#css)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [Layout & sizing](#layout-and-sizing)
    - [BEM](#bem)
    - [ID selectors](#id-selectors)
    - [JavaScript hooks](#javascript-hooks)
    - [Falsy values](#setting-falsy-values)
1. [Sass](#sass)
    - [Installation](#installation)
    - [Syntax](#syntax)
    - [Ordering](#ordering-of-property-declarations)
    - [Variables](#variables)
    - [Mixins](#mixins)
    - [Extend directive](#extend-directive)
    - [Nested selectors](#nested-selectors)


## CSS

### Formatting

* Use soft tabs (2 spaces) for indentation.
* Prefer dashes over camelCasing in class names, unless associated with JS
  - Underscores and double dashes are okay if you are using BEM (see [BEM](#bem) section below).
* Do not use ID selectors to style elements.
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations.
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line.
* Put blank lines between rule declarations.

**Bad**

```css
.seriously{
    border-radius:50%;
    border : 2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.nice {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comments

* Prefer line comments (use `//` in Sass) over block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks

**Bad**

```css
/*
Block style comment
*/
.element {
  z-index: 9999; /* end of line comment */
  color: tomato;
}
```

**Good**

```css
.element {
  /* z-index to display menu on top of everything */
  z-index: 9999;
  color: tomato;
}
```

### Layout and sizing

If you haven't already, [download the PS Sass Starter Kit](https://primitivesocial.com/ak/sass-starter-kit.zip) to get up and running with the base layout along with other goodies to speed up development.

We use the 8pt grid system (read more about it [here](https://builttoadapt.io/intro-to-the-8-point-grid-system-d2573cde8632?gi=2164ba591622)), which succinctly put, allows us to use almost exlusively `rem` units to size our layout and elements, and disallows the use of random pixel values.

**Bad**

```css
.btn {
  padding: 10px 20px;
  font-size: 18px;
}
```

**Good**

```css
.btn {
  padding: 1rem 2rem;
  font-size: 1rem;
}
```

Using this invisible grid allows our designs to feel more coherent and consistent. The only exeption to using `rem` is when working with sizes lower than `4px (.25rem)`.

> **Designers:** download [nudg.it](https://nudg.it) to modify [Sketch](https://sketchapp.com)'s *nudge* value to 8px instead of the standard 10. Or if you use [Adobe XD](https://www.adobe.com/products/xd.html), you can download [this plugin](https://github.com/littlebusters/Adjust-size-by-shortcut-for-AdobeXD) to change the nudge value.




### BEM

We encourage some combination of functional CSS and BEM for the following reasons:

  * It helps create clear, strict relationships between CSS and HTML
  * It helps us create reusable, composable components
  * It allows for less nesting and lower specificity
  * It helps in building scalable stylesheets

**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

**Example**

```css
.card { }
.card--featured { }
.card__title { }
.card__content { }
```

  * `.card` is the “block” and represents the higher-level component
  * `.card__title` is an “element” and represents a descendant of `.card` that helps compose the block as a whole.
  * `.card--featured` is a “modifier” and represents a different state or variation on the `.card` block.

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

Please create JavaScript-specific classes to bind to, prefixed with `.js-`:

```html
<button class="btn--primary js-requestToBook">Request to Book</button>
```

### Setting *falsy* values

If a property can take a number as part or all of the value, use `0` instead of `none` to specify that a style should not be displayed.

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```
**[Back to top](#table-of-contents)**


## Sass

### Installation

*Install [Dart Sass](https://github.com/sass/dart-sass/releases/tag/1.20.3) using the methods below as opposed to using [npm](https://npmjs.org) to install Sass, as the JavaScript implementation is slower than the Dart implementation. Also, [Ruby Sass](https://sass-lang.com/ruby-sass), the original implementation of Sass, has been discontinued, so if you installed Sass on your machine using the Ruby gem, please uninstall it and migrate to the Dart implementation.*

**Windows installation** using the [Chocolatey package manager](https://chocolatey.org/)

```
$ choco install sass
```

**Mac installation** using the [Homebrew package manager](https://brew.sh/)

```
$ brew install sass/sass/sass
```

### Syntax

* Use the `.scss` syntax, never the original `.sass` syntax
* Order your regular CSS and `@include` declarations logically (see below)

### Ordering of property declarations

1. Property declarations

    List all standard property declarations, anything that isn't an `@include` or a nested selector.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. `@include` declarations

    Grouping `@include`s at the end makes it easier to read the entire selector.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Extend directive

`@extend` should be used only to share styles from a base element to its alternative styles. Use `@extend` as opposed to using additional classes to override styling. See examles below.

**Bad**

```html
<button class="btn btn--ghost">Click me</button>
```

```scss
.btn {
  line-height: 3rem;
  padding: 0 2rem;
  color: $primary;
  background: seagreen;

  &.btn--ghost {
    background: transparent;
    border: 1px solid currentColor;
  }
}
```

**Good**

```html
<button class="btn--ghost">Click me</button>
```

```scss
.btn {
  line-height: 3rem;
  padding: 0 2rem;
  color: $primary;
  background: seagreen;

  &--ghost {
    @extend .btn;
    background: transparent;
    border: 1px solid currentColor;
  }
}
```

### Nested selectors

**Do not nest selectors more than three levels deep!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable


Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.

**[Back to top](#table-of-contents)**