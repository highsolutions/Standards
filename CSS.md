
# CSS

## Best practices

* Avoid specifying **global selectors** (`p`, `button`, `h1`, `[type="checkbox"]`) because this rule are too general. Use them only for normalizing and resetting default browser styles.
* Maintain low specifity in selectors (use class and tags, not IDs). But try to use them as little as possible. 
    * You can use `[id="..."]` selector instead of ID when possible.
	* Avoid going more than 3 levels deep.
* Don't chain classes - to override attribute you will need higher specificity (the same written later in code or higher).
* Use semantic class names. Classes like *.red-text*, *.blue-button*, *.border-4px* are wrong.
* Avoid connecting CSS too closely to markup:

```css
// wrong
article > h1 {

}

// slightly better
article h1 {

}

//good 
.article .article-title {

}

```

## File structure

* Basic file structure (for singular project - website/system, but not website + CMS):
    * **Libs** folder contains `__index.scss` which declares all libraries used in project and if they need to be uploaded into project assets, place these external libraries there.
	* **Helpers** folder contains mixins, functions, etc.
	* **Variables** folder contains variables used in the project like breakpoints, colors, typography, themes, etc.
	* **Base** folder contains resetting and normalizing styles.
	* **Layouts** folder contains global styles (layouts) applied to the project.
	* **Objects** folder contains styles of object (single css structures - like buttons, inputs, typography).
	* **Components** folder contains styles of components (complex css structures - like article).
	* **_Utilities** file contains simple utilities like `.u-text-center`.

```
- resources/assets/sass/
    |- libs/
    |- helpers/
    |- variables/
    |- base/
    |- layouts/
    |- objects/
    |- components/
    |- style.scss
    |- _utilities.scss
```

* `style.scss` should contain:
    * If stylesheet is not using every styles in folder, you can specify only elements you use or define it in `__index.scss` file or variaton of it `__necessary.scss`.

```sass
// External libraries
@import "libs/__index";

// Helpers
@import "helpers/__index";

// Variables
@import "variables/__index";

// Reset and base files
@import "base/__index";

// Layouts
@import "layouts/__index";

// Objects
@import "objects/__index";

// Components
@import "components/__index";

// Utilities
@import "utilities";

// Shame
@import "shame";
```

* Write rules in logic way for you, not alphabetically.
* Always write CSS mobile-first.
* Try to make your code as much readable as possible.
* Write comments about each hack and not obvious solution. 

## Sass

* Use **variables** whenever suitable.
* Use **mixins** for preventing code redundancy (like support for vendor-prefix rules), but not use them for injecting code that can be inherited.

```sass
// good:
@mixin rounded-corner($arc) {
    -moz-border-radius: $arc;
    -webkit-border-radius: $arc;
    border-radius: $arc;  
}

.tab-button {
     @include rounded-corner(5px); 
}

.cta-button {
     @include rounded-corner(8px); 
}

// bad:
@mixin cta-button {
    padding: 10px;
    color: #fff;
    background-color: red;
    font-size: 14px;
    width: 150px;
    margin: 5px 0;
    text-align: center;
    display: block;
}
```

* Use **placeholders** for injecting code that cannot be inherited. This solution not makes duplicates in code. 

```sass
%bg-image {
    width: 100%;
    background-position: center center;
    background-size: cover;
    background-repeat: no-repeat;
}

.image-one {
    @extend %bg-image;
    background-image:url("/img/image-one.jpg");
}

.image-two {
    @extend %bg-image;
    background-image:url("/img/image-two.jpg");
}

// compiles into:

.image-one, .image-two {
    width: 100%;
    background-position: center center;
    background-size: cover;
    background-repeat: no-repeat;
}

.image-one {
    background-image:url("/img/image-one.jpg") ;
}

.image-two {
    background-image:url("/img/image-two.jpg") ;
}
```

* Use **functions** to perform calculations.

```sass
@function calculate-width ($col-span) {
    @return 100% / $col-span 
}

.span-two {
    width: calculate-width(2); // spans 2 columns, width = 50%
}

.span-three {
    width: calculate-width(3); // spans 3 columns, width = 33.3%
}
```

* Use loops instead of linear code.

```sass
// Sass code
@for $i from 1 through 8 {
    $width: percentage(1 / $i)
 
    .col-#{$i} {
        width: $width;
    }
}
 
// output
.col-1 {width: 100%;}
.col-2 {width: 50%;}
.col-3 {width: 33.333%;}
.col-4 {width: 25%;}
.col-5 {width: 20%;}
.col-6 {width: 16.666%;}
.col-7 {width: 14.285%;}
.col-8 {width: 12.5%;}
```

## BEM

```sass
.block { /* styles */ }
.block__element { /* styles */ }
.block--modifier { /* styles */ }
```

* Use classes as BEM blocks, not HTML tags.

```html
<form class="form"></form>
<button class="button"></button>
```

```sass
.form { /* styles */ }
.button { /* styles */ }
```

* Modifiers are flags that change the appearance of block. Add `--modifier` to the block name.

```html
<button class="button">Primary button</button>
<button class="button button--secondary">Secondary button</button>
```

```sass
.button {
	padding: 0.5em 0.75em;
	background-color: red;
}

.button--secondary {
	background-color: green;
}
```

* Elements are children of a block. Add `__element` to the block name.

```html
<form class="form" action="">
	<div class="form__row">
		<!-- ... -->
	</div>
</form>
```

```scss
.form__row { /* styles */ }
```

* Don't chain BEM elements like `.form__row__input`. Instead:
    * Chain grandchildren elements to the block (`.form__input`).
    * Create new block within block (`.fieldset__input`).

## Namespaces
	
* Use namespaces for classes:
    * `.l-`: layouts
	* `.o-`: objects
	* `.c-`: components
	* `.js`: JavaScript hooks
	* `.is-` and `.has-`: flags
	
* **Layouts with .l-** - derived from OOCSS and SMACSS. It can be used for global layouts and block one.
    * Global layouts stored in `layouts` folder - used for site independent alignment:
	
```sass
.l-wrap {
	padding-left: 1em;
	padding-right: 1em;

	@media (min-width: 1000px) {
		max-width: 800px;
		margin-left: auto;
		margin-right: auto;
	}
}
```

```html
<header>
  <div class="l-wrap">
    <!-- stuff -->
  </div>
</header>

<footer>
  <div class="l-wrap">
    <!-- stuff -->
  </div>
</footer>
```


* Block-level layouts can be used within components or advanced blocks. Should be stored accordingly to context: in object or component file.
	
```sass
.l-form {/* container styles */}
.l-form__item {/* half-width styles */}
.l-form__item--large {/* larger-width styles */}
.l-form__item--small {/* smaller-width styles */}
```

```html
<form class="form l-form" action="#">
	<div class="form__row">
		<div class="form__item l-form__item"></div>
		<div class="form__item l-form__item"></div>
	</div>
	<div class="form__row">
		<div class="form__item l-form__item--large"></div>
		<div class="form__item l-form__item--small"></div>
	</div>
</form>
```	

* **Objects with .o-** - derived from Atomic CSS, smallest final building blocks of a website. They cannot contain other objects or components and are context independent, but they can contain inner elements.
    * Objects are context independent so they cannot change external structure of website - only static position, no margin, padding, float, etc.

```html
<a href="#" class="o-button">A button</a>

<div class="o-logo">
	<div class="o-logo__image">
		<img src="" alt="Company logo">
	</div>
	<div class="o-logo__text--bigger">Text</div>
</div>
```

* **Components with .c-** - derived from Atomic CSS, larger building blocks. They can contain other objects and components and are context aware.

```html
<form class="c-form l-form" action="#">
	<div class="c-form__row">
		<div class="c-form__item l-form__item"></div>
		<div class="c-form__item l-form__item"></div>
	</div>
	<div class="form__row">
		<div class="c-form__item l-form__item--large"></div>
		<div class="c-form__item l-form__item--small"></div>
	</div>
	<div class="form__row">
		<a href="#" class="o-button c-form__button">Submit button</a>
	</div>
</form>

<!-- context aware version -->
<form class="c-form c-form--sidebar l-form" action="#">
	<div class="c-form__row">
		<div class="c-form__item l-form__item"></div>
		<div class="c-form__item l-form__item"></div>
	</div>
	<div class="form__row">
		<div class="c-form__item l-form__item"></div>
		<div class="c-form__item l-form__item"></div>
	</div>
	<div class="form__row">
		<a href="#" class="o-button c-form__button">Submit button</a>
	</div>
</form>
```	

* **JavaScript hooks with .js** - indicates if an object/component requires JavaScript code.

```html
<form class="c-form l-form jsFormValidation" action="#">
	<!-- ... -->
</form>
```

* **Flags with .is- and .has-** - derived from SMACSS, indicate the current state of the object/component.

```sass
.object {
  &.is-open { /* styles */}
}

.object {
  &.has-dropdown { /* styles */}
}
```
	
Further reading:
* [Golden Guidelines for Writing Clean CSS](https://www.sitepoint.com/golden-guidelines-for-writing-clean-css/)
* [Sass Guidelines](https://sass-guidelin.es/)
* [Writing modular CSS (Part 1) - BEM](https://zellwk.com/blog/css-architecture-1/)
* [Writing modular CSS (Part 2) - Namespaces](https://zellwk.com/blog/css-architecture-2/)
* [Writing modular CSS (Part 3) - File structures](https://zellwk.com/blog/css-architecture-3/)

## Flexbox

* DO NOT use flexbox for page layout. Use CSS Grid or typical approach with percentages, max-width and media queries.
* DO NOT add `display: flex;` to every single container. Add it only when suitable.
* Use flexbox primary for: scaling, alignment and order.