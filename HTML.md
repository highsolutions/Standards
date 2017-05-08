
# HTML & Blade

## HTML structure

* Use semantics of HTML to describe content:

```html
<!-- bad -->
<div id="main">
  <div class="article">
    <div class="header">
      <h1>Blog post</h1>
      <p>Published: <span>21st Feb, 2015</span></p>
    </div>
    <p>…</p>
  </div>
</div>

<!-- good -->
<main>
  <article>
    <header>
      <h1>Blog post</h1>
      <p>Published: <time datetime="2015-02-21">21st Feb, 2015</time></p>
    </header>
    <p>…</p>
  </article>
</main>
```

* Use HTML5 syntax, not XHTML:

```html
<!-- bad -->
<!doctype html>
<html lang="en:>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title>Contact</title>
    <link rel="stylesheet" href="style.css" type="text/css" />
  </head>
  <body>
    <h1>Contact me</h1>
    <label>
      Email address:
      <input type="email" placeholder="you@email.com" required="required" />
    </label>
    <script src="main.js" type="text/javascript"></script>
  </body>
</html>

<!-- good -->
<!doctype html>
<html lang="en">
  <meta charset="utf-8">
  <title>Contact</title>
  <link rel="stylesheet" href="style.css">

  <h1>Contact me</h1>
  <label>
    Email address:
    <input type="email" placeholder="you@email.com" required>
  </label>
  <script src="main.js"></script>
</html>
```

* Use basic accessibility features:
    * Use descriptive `alt` attributes.
	* Links should be `<a>` and buttons should be `<button>` (not `<div class="button"></div>`).
	* No rely only on colors to communicate information.
	* Form controls have to have labels.

* Define language of HTML document and use UTF-8 encoding always.

* Assets order in document:
    * One stylesheet only, but if it's heavy, split them into two: first with most basic styles, and second with all of styles with deferred loading.
	* All javascript scripts should be placed at the end of the document.
	* Ideally there should be one internal javascript script tag.
	
## Blade

* Use layouts as a base of design.
* Use `{{ }}` when possible, and `{!! !!}` only when you are sure that users won't use it for XSS attack.
* Use Blade **control structures** instead of PHP native ones.
    * Use `@unless`, `@isset`, `@empty` when suitable instead of typical `@if`.
	* Use `@forelse` loop when you want to show all elements of Collection and handle situation when there can be no elements in it.
	* Use `@continue(condition)` and `@break(condition)` to control loops.
	* Use `$loop` variable every time is suitable (for checking if it's a first or last record, what iteration of loop currently is, how many elements left, etc.).
* When you need use PHP code, use `@php ... @endphp` structure. 
* Use `@each` control structure instead of:

```php
{{-- old way --}}
@forelse($articles as $article)
	@include('view.name', ['article' => $article])
@empty
	@include('view.empty')
@endforeach

{{-- new way (fourth parameter is optional) --}}
@each('view.name', $articles, 'article', 'view.empty')
```

* Use `@lang` directive when you can display translation file with escaping brackets.
    * Remember to use keys in lang files lowercased, unique, and explanatory and store them alphabetically.
	* Split language translations into files based on domain separation.
	
* Split views into smaller ones using `@include` for bigger readability, maintainability. 

```
articles
|-- article
|   |-- header.blade.php
|   |-- text.blade.php
|   |-- author.blade.php
|   |-- similar.blade.php
|-- list
|   |-- record.blade.php
|-- article.blade.php
|-- list.blade.php
```

* You can also extend Blade directives when you need helper in views all the time.