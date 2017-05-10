# JS, jQuery, Vue.js

## JavaScript style guide

Based on [https://github.com/airbnb/javascript](Airbnb JavaScript Style Guide).

### Types and variables

* **Primitives** - when you access a primitive type (`string`, `number`, `boolean`, `null`, `undefined`), you work directly on its value.

```js
const foo = 1;
let bar = foo;

bar = 9;

console.log(foo, bar); // => 1, 9
```

* **Complex** - when you access a complex type (`object`, `array`, `function`), you work on a reference to its value.

```js
const foo = [1, 2];
const bar = foo;

bar[0] = 9;

console.log(foo[0], bar[0]); // => 9, 9
```

* Use `const` for all of your variables to ensure that you can't reassign them.

```js
// bad
var a = 1;
var b = 2;

// good
const a = 1;
const b = 2;
```

* Use `let` when you have to reassign references (instead of `var`).

```js
// bad
var count = 1;
if (true) {
  count += 1;
}

// good, use the let.
let count = 1;
if (true) {
  count += 1;
}
```

* Use one `const` or `let` declaration per variable.

```js
// bad
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

// bad
// (compare to above, and try to spot the mistake)
const items = getItems(),
    goSportsTeam = true;
    dragonball = 'z';

// good
const items = getItems();
const goSportsTeam = true;
const dragonball = 'z';
```

* Group all your `const`s and then group all your `let`s.

```js
// bad
let i, len, dragonball,
    items = getItems(),
    goSportsTeam = true;

// bad
let i;
const items = getItems();
let dragonball;
const goSportsTeam = true;
let len;

// good
const goSportsTeam = true;
const items = getItems();
let dragonball;
let i;
let length;
```

* Use `===` and `!==` over `==` and `!=`.

* Use shortcuts for booleans, but explicit comparisons for strings and numbers.

```js
// bad
if (isValid === true) {
  // ...
}

// good
if (isValid) {
  // ...
}
```

```js
// bad
if (name) {
  // ...
}

// good
if (name !== '') {
  // ...
}
```

```js
// bad
if (collection.length) {
  // ...
}

// good
if (collection.length > 0) {
  // ...
}
```

* Ternaries should not be nested and generally be single line expressions.

```js
// bad
const foo = maybe1 > maybe2
  ? "bar"
  : value1 > value2 ? "baz" : null;

// better
const maybeNull = value1 > value2 ? 'baz' : null;

const foo = maybe1 > maybe2
  ? 'bar'
  : maybeNull;

// best
const maybeNull = value1 > value2 ? 'baz' : null;

const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
```

* Use braces with all multi-line blocks.

```js
// bad
if (test)
  return false;

// good
if (test) return false;

// good
if (test) {
  return false;
}

// bad
function foo() { return false; }

// good
function bar() {
  return false;
}
```

* If you're using multi-line blocks with if and else, put else on the same line as your if block's closing brace. 

```js
// bad
if (test) {
  thing1();
  thing2();
}
else {
  thing3();
}

// good
if (test) {
  thing1();
  thing2();
} else {
  thing3();
}
```

* Use semicolons at the end of expressions.

```js
// bad
(function () {
  const name = 'Skywalker'
  return name
})()

// good
(function () {
  const name = 'Skywalker';
  return name;
}());
```

* Perform type coercion at the beginning of the statement. Strings:

```js
// bad
const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

// bad
const totalScore = this.reviewScore.toString(); // isn't guaranteed to return a string

// good
const totalScore = String(this.reviewScore);
```

* Numbers: Use `Number` for type casting and `parseInt` always with a radix for parsing strings.

```js
const inputValue = '4';

// bad
const val = new Number(inputValue);

// bad
const val = +inputValue;

// bad
const val = inputValue >> 0;

// bad
const val = parseInt(inputValue);

// good
const val = Number(inputValue);

// good
const val = parseInt(inputValue, 10);
```

* Booleans

```js
const age = 0;

// bad
const hasAge = new Boolean(age);

// good
const hasAge = Boolean(age);

// best
const hasAge = !!age;
```

* Use camelCase when naming objects, functions, and instances.

```js
// bad
const OBJEcttsssss = {};
const this_is_my_object = {};
function c() {}

// good
const thisIsMyObject = {};
function thisIsMyFunction() {}
```

* Use PascalCase only when naming classes.

```js
// bad
function user(options) {
  this.name = options.name;
}

const bad = new user({
  name: 'nope',
});

// good
class User {
  constructor(options) {
    this.name = options.name;
  }
}

const good = new User({
  name: 'yup',
});
```

* If the property/method is a `boolean`, use `isVal()` or `hasVal()`.

```js
// bad
if (!dragon.age()) {
  return false;
}

// good
if (!dragon.hasAge()) {
  return false;
}
```

### Objects

* Use literal syntax for object creation.

```js
// bad
const item = new Object();

// good
const item = {};
```

* Use property value shorthand. And group your shorthand properties at the beginning of your object declaration.

```js
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
  lukeSkywalker: lukeSkywalker,
  darthVader: "Darth Vader"
};

// good
const obj = {
  lukeSkywalker,
  darthVader: "Darth Vader"
};
```

* Only quote properties that are invalid identifiers.

```js
// bad
const bad = {
  'foo': 3,
  'bar': 4,
  'data-blah': 5,
};

// good
const good = {
  foo: 3,
  bar: 4,
  'data-blah': 5,
};
```

### Arrays

* Use the literal syntax for array creation.

```js
// bad
const items = new Array();

// good
const items = [];
```

* Use `Array@push` instead of direct assignment to add items to an array.

```js
const someStack = [];

// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```

* Use array spreads ... to copy arrays.

```js
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i += 1) {
  itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
```

* Use line breaks after open and before close array brackets if an array has multiple lines.

```js
// bad
const arr = [
  [0, 1], [2, 3], [4, 5],
];

const objectInArray = [{
  id: 1,
}, {
  id: 2,
}];

const numberInArray = [
  1, 2,
];

// good
const arr = [[0, 1], [2, 3], [4, 5]];

const objectInArray = [
  {
    id: 1,
  },
  {
    id: 2,
  },
];

const numberInArray = [
  1,
  2,
];
```

### Strings

* Use single quotes `''` for strings.

```js
// bad
const name = "Capt. Janeway";

// bad - template literals should contain interpolation or newlines
const name = `Capt. Janeway`;

// good
const name = 'Capt. Janeway';
```

* Strings that cause the line to go over 100 characters should not be written across multiple lines using string concatenation.

```js
// bad
const errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// bad
const errorMessage = 'This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.';

// good
const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
```

* When programmatically building up strings, use template strings instead of concatenation.

```js
// bad
function sayHi(name) {
  return 'How are you, ' + name + '?';
}

// bad
function sayHi(name) {
  return ['How are you, ', name, '?'].join();
}

// bad
function sayHi(name) {
  return `How are you, ${ name }?`;
}

// good
function sayHi(name) {
  return `How are you, ${name}?`;
}
```

* Never use `eval()` on a string, it opens too many vulnerabilities.

### Functions

* Use named function expressions instead of function declarations.

```js
// bad
function foo() {
  // ...
}

// bad
const foo = function () {
  // ...
};

// good
const foo = function bar() {
  // ...
};
```

* Never declare a function in a non-function block (`if`, `while`, etc). Assign the function to a variable instead.

```js
// bad
if (currentUser) {
  function test() {
    console.log('Nope.');
  }
}

// good
let test;
if (currentUser) {
  test = () => {
    console.log('Yup.');
  };
}
```

* Use default parameter syntax rather than mutating function arguments.

```js
// really bad
function handleThings(opts) {
  // No! We shouldn't mutate function arguments.
  // Double bad: if opts is falsy it'll be set to an object which may
  // be what you want but it can introduce subtle bugs.
  opts = opts || {};
  // ...
}

// still bad
function handleThings(opts) {
  if (opts === void 0) {
    opts = {};
  }
  // ...
}

// good
function handleThings(opts = {}) {
  // ...
}
```

* Spacing in a function signature.

```js
// bad
const f = function(){};
const g = function (){};
const h = function() {};

// good
const x = function () {};
const y = function a() {};
```

* Never reassign parameters.

```js
// bad
function f1(a) {
  a = 1;
  // ...
}

function f2(a) {
  if (!a) { a = 1; }
  // ...
}

// good
function f3(a) {
  const b = a || 1;
  // ...
}

function f4(a = 1) {
  // ...
}
```

### Lambda functions

* When you must use function expressions (as when passing an anonymous function), use arrow function notation. 

```js
// bad
[1, 2, 3].map(function (x) {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```

### Classes

* Always use `class`. Avoid manipulating `prototype` directly.

```js
// bad
function Queue(contents = []) {
  this.queue = [...contents];
}
Queue.prototype.pop = function () {
  const value = this.queue[0];
  this.queue.splice(0, 1);
  return value;
};


// good
class Queue {
  constructor(contents = []) {
    this.queue = [...contents];
  }
  pop() {
    const value = this.queue[0];
    this.queue.splice(0, 1);
    return value;
  }
}
```

* Use `extends` for inheritance.

```js
// bad
const inherits = require('inherits');
function PeekableQueue(contents) {
  Queue.apply(this, contents);
}
inherits(PeekableQueue, Queue);
PeekableQueue.prototype.peek = function () {
  return this.queue[0];
};

// good
class PeekableQueue extends Queue {
  peek() {
    return this.queue[0];
  }
}
```

* Methods can return `this` to help with method chaining.

```js
// bad
Jedi.prototype.jump = function () {
  this.jumping = true;
  return true;
};

Jedi.prototype.setHeight = function (height) {
  this.height = height;
};

const luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20); // => undefined

// good
class Jedi {
  jump() {
    this.jumping = true;
    return this;
  }

  setHeight(height) {
    this.height = height;
    return this;
  }
}

const luke = new Jedi();

luke.jump()
  .setHeight(20);
```

* It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

```js
class Jedi {
  constructor(options = {}) {
    this.name = options.name || 'no name';
  }

  getName() {
    return this.name;
  }

  toString() {
    return `Jedi - ${this.getName()}`;
  }
}
```

* Classes have a default constructor if one is not specified. An empty constructor function or one that just delegates to a parent class is unnecessary.

```js
// bad
class Jedi {
  constructor() {}

  getName() {
    return this.name;
  }
}

// bad
class Rey extends Jedi {
  constructor(...args) {
    super(...args);
  }
}

// good
class Rey extends Jedi {
  constructor(...args) {
    super(...args);
    this.name = 'Rey';
  }
}
```

### Modules

* Always use modules (`import`/`export`) over a non-standard module system. You can always transpile to your preferred module system.

```js
// bad
const AirbnbStyleGuide = require('./AirbnbStyleGuide');
module.exports = AirbnbStyleGuide.es6;

// ok
import AirbnbStyleGuide from './AirbnbStyleGuide';
export default AirbnbStyleGuide.es6;

// best
import { es6 } from './AirbnbStyleGuide';
export default es6;
```

* Do not use wildcard imports.

```js
// bad
import * as AirbnbStyleGuide from './AirbnbStyleGuide';

// good
import AirbnbStyleGuide from './AirbnbStyleGuide';
```

### Iterators

* Don't use iterators. Prefer JavaScript's higher-order functions instead of loops like `for-in` or `for-of`.
    * Use `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... to iterate over arrays.
	* Use `Object.keys()` / `Object.values()` / `Object.entries()` to produce arrays so you can iterate over objects.

```js
const numbers = [1, 2, 3, 4, 5];

// bad
let sum = 0;
for (let num of numbers) {
  sum += num;
}
sum === 15;

// good
let sum = 0;
numbers.forEach(num => sum += num);
sum === 15;

// best (use the functional force)
const sum = numbers.reduce((total, num) => total + num, 0);
sum === 15;
```

```js
// bad
const increasedByOne = [];
for (let i = 0; i < numbers.length; i++) {
  increasedByOne.push(numbers[i] + 1);
}

// good
const increasedByOne = [];
numbers.forEach(num => increasedByOne.push(num + 1));

// best (keeping it functional)
const increasedByOne = numbers.map(num => num + 1);
```

### Properties

* Use dot notation when accessing properties.

```js
const luke = {
  jedi: true,
  age: 28,
};

// bad
const isJedi = luke['jedi'];

// good
const isJedi = luke.jedi;
```

* Use bracket notation `[]` when accessing properties with a variable.

```js
const luke = {
  jedi: true,
  age: 28,
};

function getProp(prop) {
  return luke[prop];
}

const isJedi = getProp('jedi');
```

## jQuery

### Initializing

* Loading jQuery in HTML.

```html
<script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.js"></script>
```

* Prefix jQuery object variables with a `$`.

```js
// bad
const sidebar = $('.sidebar');

// good
const $sidebar = $('.sidebar');

// good
const $sidebarBtn = $('.sidebar-btn');
```

* Start jQuery code in IIFE.

```js
// IIFE - Immediately Invoked Function Expression
(function($, window, document) {

	// The $ is now locally scoped 

	// Listen for the jQuery ready event on the document
	$(function() {

		// The DOM is ready!

	});

	// The rest of the code goes here!

}(window.jQuery, window, document));
// The global jQuery object is passed as a parameter
```

* Document ready event handlers should be included from external files and inline JavaScript can be used to call the ready handle after any initial setup.

```js
// my-document-ready-function.js
function initPage() {
	// Initializing values, callbacks, events, etc.
}
```

```html
<script src="my-document-ready-function.js"></script>
<script>
(function($, window, document) {

	$(initPage);

}(window.jQuery, window, document));
</script>
```

### Selectors

* Use ID selector whenever possible.

* When using class selectors, don't use the element type in your selector.

```js
// slow
const $products = $('div.products');

// fast
const $products = $('.products');
```

* Use `find()` for ID > Child nested selectors.

```js
// slow
const $productIds = $('#products div.id');

// fast
const $productIds = $('#products").find('div.id');
``

* Be specific on the right-hand side of your selector, and less specific on the left.

```js
// unoptimized
$('div.data .gonzalez');

// optimized
$('.data td.gonzalez');
```

* Avoid excessive specificity.

```js
$('.data table.attendees td.gonzalez');
 
// Better: Drop the middle if possible.
$('.data td.gonzalez');
```

* Give your selectors a context.

```js
// slow
$('.class');

// fast
$('.class', '#class-container');

$('#class-container').find('.class');
```

* Avoid universal selectors.

```js
// bad
$('div.container > *');

// good
$('div.container').children();
```

* Avoid implied universal selectors. When you leave off the selector, the universal selector (*) is still implied.

```js
// bad
$('div.someclass :radio');

// good
$('div.someclass input:radio');
```

* Don’t descend multiple IDs or nest when selecting an ID.

```js
// bad
$('#outer #inner');
$('div#inner');
$('.outer-container #inner');

// good
$('#inner');
```

### DOM

* Cache jQuery lookups.

```js
// bad
function setSidebar() {
  $('.sidebar').hide();

  // ...

  $('.sidebar').css({
    'background-color': 'pink',
  });
}

// good
function setSidebar() {
  const $sidebar = $('.sidebar');
  $sidebar.hide();

  // ...

  $sidebar.css({
    'background-color': 'pink',
  });
}
```

* For DOM queries use cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. And use `find` with scoped jQuery object queries.

```js
// bad
$('ul', '.sidebar').hide();

// bad
$('.sidebar').find('ul').hide();

// good
$('.sidebar ul').hide();

// good
$('.sidebar > ul').hide();

// good
$sidebar.find('ul').hide();
```

* Always detach any existing element before heavy manipulation and attach it back after manipulating it.

```js
let $myList = $("#list-container > ul").detach();
// ...a lot of complicated things on $myList
$myList.appendTo("#list-container");
```

* Use string `concatenation` or `array.join()` over `.append()`.

```js
// bad
let $myList = $('#list');
for(let i = 0; i < 10000; i++) {
    $myList.append('<li>' + i + '</li>');
}
 
// good
let $myList = $("#list");
let list = "";
for(let i = 0; i < 10000; i++) {
    list += '<li>' + i + '</li>';
}
$myList.html(list);
 
// best
let array = []; 
for(let i = 0; i < 10000; i++) {
    array[i] = '<li>${i}</li>'; 
}
$('#list').html(array.join(''));
```

* Don’t act on absent elements.

```js
// bad -> This runs three functions before it realizes there's nothing in the selection
$("#nosuchthing").slideUp();
 
// good
const $mySelection = $("#nosuchthing");
if ($mySelection.length) {
    $mySelection.slideUp();
}
```

* Use object literals for parameters.

```js
// bad
$myLink.attr('href', '#').attr('title', 'my link').attr('rel', 'external');

// good
$myLink.attr({
    href: '#',
    title: 'my link',
    rel: 'external'
});
```

* Use `.prop()` instead of `.attr()` when accessing properties of DOM elements.
    * To set property as false, use `.prop('name', false)` instead of `.removeProperty()`.
	* Use for properties like `checked`, `selected`, etc. Mainly these which can be changed by user.
	* Use `.val()` for accessing value of element.

```js
// bad
$('input:checked').attr('checked'); // --> checked

// good
$('input:checked').prop('checked'); // --> true
```

### Events

* Use only one Document Ready handler per page.

* Do not use anonymous functions to attach events.

```js
// bad
$('#myLink').on('click', function() {...});
 
// good
function myLinkClickHandler() {...}
$('#myLink').on('click', myLinkClickHandler);
```

* Event handling and delegation moved to variable and handle children.

```js
// bad
$("#longlist li").on("mouseenter", function() {
	$(this).text("Click me!");
});

$("#longlist li").on("click", function() {
	$(this).text("Why did you click me?!");
});
  
// good
let $list = $("#longlist");

$list.on("mouseenter", "li", function(){
	$(this).text("Click me!");
}).on("click", "li", function() {
	$(this).text("Why did you click me?!");
});
```

* DO NOT use event handlers in HTML.

```html
<!-- bad -->
<a id="myLink" href="#" onclick="myEventHandler();">my link</a>
```

```js
// good
$('#myLink').on('click', myEventHandler);
```

###  AJAX

* Avoid using `.getJson()` or `.get()`, simply use the `$.ajax()` as that's what gets called internally.

* DO NOT put request parameters in the URL, send them using data object setting.

```js
// bad
$.ajax({
    url: 'something.php?param1=test1&param2=test2',
    ....
});
 
// good
$.ajax({
    url: 'something.php',
    data: { param1: test1, param2: test2 }
});
```

* Use promises for ajax requests.

```js
function getName(personid) {
	const dynamicData = {
		id: personid
	};
	
	return $.ajax({
		url: "SOME_URL",
		type: "GET",
		data: dynamicData
	});
}

getName("2342342").done(function(data) {
	// Updates the UI based the ajax result
	$(".person-name").text(data.name); 
});
```

* Sample ajax template.

```js
$.ajax({
    url: url,
    type: "GET",
    cache: true,
    data: {},
    dataType: "json",
    statusCode: {
        404: handler404,
        500: handler500
    }
}).done(successHandler)
	.fail(failureHandler);
```

### Plugins

* Any common reusable component should be implemented as a jQuery plugin and stored in separate JS file.

```js
(function($) {
 
    $.pluginName = function(element, options) {
 
        const defaults = { 
            foo: 'bar',
            onFoo: function() {} 
        } 
        const plugin = this; 
        const $element = $(element);
        const element = element;
 
        plugin.settings = {}
 
        plugin.init = function() {
            plugin.settings = $.extend({}, defaults, options);
 
            // code goes here 
        }
 
        // a public method
        plugin.foo_public_method = function() {
 
            // code goes here
 
        }
  
        // a private method
        const foo_private_method = function() {
 
            // code goes here
 
        }
 
        plugin.init();
		
    }
 
    $.fn.pluginName = function(options) {
 
        return this.each(function() { 
            if($(this).data('pluginName') !== undefined)
				return;
 
			const plugin = new $.pluginName(this, options);

			$(this).data('pluginName', plugin);  
        });
 
    }
 
})(jQuery);
```

* Plugin's properties will be available through this object like:
	
```js
// inside the plugin
plugin.settings.propertyName

// outside of plugin
$element.data('pluginName').settings.propertyName
```

* Plugin's public method will be available through this object like:

```js
// inside the plugin
plugin.methodName(arg1, arg2, ... argn)

// outside of plugin
$element.data('pluginName').publicMethod(arg1, arg2, ... argn)
```

* Plugin's private method will be available through this object like:

```js
// inside the plugin
methodName(arg1, arg2, ... argn)
```

## Vue.js

Coming soon...