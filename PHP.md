# PHP

We're using [PSR-2 Coding Standard](https://docs.opnsense.org/development/guidelines/psr2.html), except cases stated explicitly different.

All good practices are mentioned in [PHP: The Right Way](http://www.phptherightway.com/).

Clean code is also very important and good practices are mentioned in [Clean Code PHP](https://github.com/jupeter/clean-code-php).

## Overview of PSR-2

* There MUST be one blank line after the namespace declaration, and there MUST be one blank line after the block of use declarations.
* Opening braces for classes MUST go on the next line, and closing braces MUST go on the next line after the body.
* Opening braces for methods MUST go on the next line, and closing braces MUST go on the next line after the body.
* Opening parentheses for control structures MUST NOT have a space after them, and closing parentheses for control structures MUST NOT have a space before.
* Use declarations SHOULD be in alphabetical order.

Example:

```php
<?php

namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface
{

    public function sampleFunction($a, $b = null)
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($a);
        } else {
            BazClass::bar($a, $b);
        }
    }

    final public static function bar()
    {
        // method body
    }

}
```

### Lines

* All PHP files MUST use the Unix LF (linefeed) line ending. All PHP files MUST end with a single blank line.
* The closing ?> tag MUST be omitted from files containing only PHP.
* There MUST NOT be trailing whitespace at the end of non-blank lines.
* Each indentation MUST have 4 spaces characters

### Keywords 

* PHP keywords MUST be in lower case.
* The PHP constants `true`, `false`, and `null` MUST be in lower case.

### Classes

* Class names MUST be declared in `StudlyCaps`.
* The extends and implements keywords MUST be declared on the same line as the class name.
* The opening brace for the class MUST go on its own line; the closing brace for the class MUST go on the next line after the body.
* Class constants MUST be declared in all upper case with underscore separators.
* Always have a single newline at the beginning and end of each class/trait, resulting from a newline above and below each method.

### Properties

* Property names SHOULD be declared in `camelCase`
 
Example:

```php
<?php

namespace Vendor\Package;

class Foo
{

    public $fooBar = null;

}
```

### Methods

* Method names MUST be declared in `camelCase`
* In the argument list, there MUST NOT be a space before each comma, and there MUST be one space after each comma.

Example: 

```php
<?php

namespace Vendor\Package;

class ClassName
{

    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }

}
```

Method chaining should have multiple methods spread across separate lines, and indented with one tab:

    $email->from('foo@example.com')
        ->to('bar@example.com')
        ->subject('A great message')
        ->send();

### Quotes

If it is pure text use single quotes, but if the text contains variables use double quotes.

This is used in the performance sense, the Compiler will handle single quotes faster.

### Casting variables

For casting use:

* `(bool)` - Cast to boolean.
* `(int)` - Cast to integer.
* `(float)` - Cast to float.
* `(string)` - Cast to string.
* `(array)` - Cast to array.
* `(object)` - Cast to object.

### `if`, `elseif`, `else`

```php
<?php
if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
```

* The keyword `elseif` SHOULD be used instead of else if so that all control keywords look like single words.
* Always try to be as strict as possible. If a non-strict test is deliberate it might be wise to comment it as such to avoid confusing it for a mistake. 

The value to check against should be placed on the right side:

```php
<?php

// Faster and easier than is_null() call
if ($value === null) {
    // ...
}
```

### `switch`, `case`

```php
<?php

switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
```

* There MUST be a comment such as // no break when fall-through is intentional in a non-empty case body.

### Loops

```php
<?php

while ($expr) {
    // structure body
}

do {
    // structure body;
} while ($expr);

for ($i = 0; $i < 10; $i++) {
    // for body
}

foreach ($iterable as $key => $value) {
    // foreach body
}
```

### Strings

`'` or `"`? Both work, as long as they are used consistent throughout a file. It is recommended to use the single `'` - as `"` is for HTML attributes and parses variables.

Don't use variables inside strings - they are better spitted like that:

```php
echo 'A string with ' . $someVariable . ' and ' . SOME_CONSTANT . '!';
echo '<a href="example.org" title="' . $title . '">Link</a>';
```

In case a string contains `'`, it is applicable to switch to ``" here to avoid the usage of `\` escapes:

```php
$sql = "UPDATE TABLE 'foo' SET ContactName='Alfred Schmidt', City='Hamburg' WHERE ...";
```

Use a space before and after `.`:

```php
$myString = 'a string' . $variable . 'more string stuff etc';
```

All operators should go in the newline as first character:

```php
$foo = 'Some String'
    . ' concatinated';
```

### Arrays

Arrays that span across multiple lines can have a trailing comma to make sure that adding new rows does not change the previous row, as well.

```php
$array = [1, 2, 3, 4, 5];

$array = [
    'first' => [
        'first-A' => 0,
        'first-B' => [1, 2, 3, 4],
    ],
    'second',
    'third' => ['12', '34', '56'],
];
```

### Comments

Inline code should use // and a single space afterwards. Use sentences with a capital first letter and a full stop if possible:

```php
// This is a nice inline comment.
$this->doSomehing();
```

Recommended tags for each function/method are (in this order):

* `@param` if applicable
* `@return`
* `@throws` if applicable
* `@triggers` if applicable

Example:

```php
/**
 * Returns output of input.
 *
 * @param int|bool $input Input as integer or boolean value.
 * @return int|bool Output pretty much the same.
 */
public function foo($input) {
    return $input;
}
```

### Exceptions

```php
<?php

try {
    // try body
} catch (FirstExceptionType $e) {
    // catch body
} catch (OtherExceptionType $e) {
    // catch body
}
```

### Closures

```php
<?php

$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};
```

### Functional programming > Structural programming

One of the strengths of php is the possibility of functional programming. Use them!

Example:

```php
//bad
function foo(): array
{
    $names = [
        'adam',
        'maria',
        'tom'
    ];

    $upperNames = [];
    
    foreach($names as $name) {
        $upperNames[] = strtoupper($name);
    }

    return $upperNames; 
}

//good

function foo(): array
{
    $names = [
        'adam',
        'maria',
        'tom'
    ];

    return array_map( function($name) {
        return strtoupper($name);
    }, $names);
}

```

#### More functions:
* [array_filter](https://www.php.net/manual/en/function.array-filter.php)
* [array_reduce](https://www.php.net/manual/en/function.array-reduce.php)
* [array_walk](https://www.php.net/manual/en/function.array-walk.php)

#### More information about functional programming in PHP
* [Great first step article](https://apiumhub.com/tech-blog-barcelona/functional-php/)