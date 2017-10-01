# Laravel

Follow Style Guide for [PHP](/highsolutions/Standards/wiki/PHP).

## Installation

* Keep in mind server requirements and choose the newest version of Laravel that fulfills PHP version on production server.
* Configure .env.server file to be ready to just rename it to .env on server.
* Configure GitHub hook to Continuous Integration on every push.
* Use *.dev or *.local developer domain on your local machine.
* Do not store `.env` file, `storage` folder, and `vendor` folder in repo.

## Configuration

* Use `.env` files for every setting of application that can vary between developers computers, staging server and production server.
* Use config files to define every setting for application that is constant. Use .env variables when necessary.
* Config files must use `kebab-case`, when keys `snake_case`.
* Do not store API keys etc. inside code. Put them into .env or config files.
* Dynamic settings store in database or in translation files (if depend on localization).
* Use `config()` or `\Config::get()` methods instead of accessing configuration from `env()` method. 

## Controllers

* Organize controllers in subfolders when application consists of more than one module (e.g. Website, Admin, Auth).
* Use [Resource Controllers](https://laravel.com/docs/controllers#resource-controllers) when possible and method names from this convention (`index`, `show`, `create`, `store`, `edit`, `update`, `destroy`). 
	* This controllers must be named in plurar case.
* Do not use other methods on controller. If necessary, create new controller and follow convention. 
	* E.g. when you have `favorite()` and `unfavorite()` methods, extract them to `FavoritesController` and use `store` and `destroy` methods.
* Controllers for admin panel create using `Route::resource`. Use parameters `only` and `except` when necessary.
* Methods should have only few responsibilities:
  * Displaying data
	  * Fetch data from repositories/services
	  * Display view with data

Example:

```php
/**
 * Display list of posts of category
 *
 * @return Response
 */
public function show($category)
{
    $posts = $this->repo->getByCategory($category);

    return view('blog/posts', compact('posts'));
}
```

  * Receiving data
		  * Authorization of request
		  * Validation of request's input
		  * Send input to Repository/Service and receive data/status
		  * Display view with required data or return response with data

Example:

```php
/**
 * Store a new blog post.
 *
 * @param  Request  $request
 * @return Response
 */
public function store(Request $request)
{	
	$this->authorize('create');
	
    $this->validate($request, [
        'title' => 'required|unique:posts|max:255',
        'body' => 'required',
    ]);

    $status = $this->repo->store(request());

    return response()->json(['status' => $status]);
}   
``` 
 
  * With more advanced request authorization and/or validation, [FormRequest](https://laravel.com/docs/5.4/validation#form-request-validation) should be used.
  * When method should act differently based on parameters/input data, use private methods to keep method clean and short.
  * When controller has to send many static data to view (not based on parameters), use [ViewComposer](https://laravel.com/docs/views#view-composers).
  * When methods of controllers are complicated, extract each method into [RequestHandler](https://jenssegers.com/85/goodbye-controllers-hello-request-handlers) (Action-Domain-Responder pattern). 

## Services, Traits and Packages

* Extend logic of your code in three ways:
  * **Services** - when you need simple functionality from the black-box
  * **Traits** - when you want to extend Model/Controller functionality
  * **Packages** - when you need full module (from vendor or your own)
* Use Service Providers and power of Illuminate Container

## Helpers and extending core

* Use helper methods only for views and routes and only when helper method will be reusable in many places.
* Protect helpers functions before duplicating, use SNAKE_CASE and write comments:

```php
if (!function_exists('compare_floats')) {
	/**
	 * Compare float values ignoring rounding issues
	 * 
	 * @param float $a
	 * @param float $b
	 * @return bool
	 */
	function compare_floats($a, $b)
	{
		return abs($a - $b) < 0.001;
	}
}
```

* Define Validator and Blade extensions in AppServiceProvider (even better keep implementation as Service for easier maintenance).

## Routing

* Every route has to be bound with controller method. Use closures only for testing.
* Use regular expression constraints for parameters.
* Name every route. Base on Resource Controller convention.
* Do not use in views or controllers `\URL::to()` regarding routes within application. Use `route(...)`.
* Group routes logically and set namespace parameter when using catalogs for controllers.
* Use prefixes in group routes instead of duplicate them in route url.

## Views

* View files must be named in `camelCase`.
* Orgniaze views accordingly to controllers structure.
* Divide Blade files into smaller partials.
* Make components files reusable.
* Don't use any unnecessary logic in view file.
* Use `{{ __('file.key') }}` for translations when no using `transEditable` method.

## Commands

* Write commands for operations which are launched only by developers in console.
* Write descriptions to know what command do and always returns information about finishing operation.
* Names of commands should be `kebab-case`.
* Use commands for cron operations via Artisan Scheduler.

## Cache

* Use cache when system returns many dynamic data that are rarely changed or when system will be heavily used.
* Use redis when possible.
* Hide cache implementation in repository/services, not use it in controllers.
* Don't use cache in local development (use `.env` to enable/disable), except cache testing.

## Errors

* Store logs in daily mode (not store in one tremendous laravel.log).
* Always prepare 404 error page.
* When other error, redirect to back or main view (homepage/dashboard) and display error with explanation. You can override default behavior in `App\Exceptions\Handler@render`.
* In staging and production use [bugsnag](https://www.bugsnag.com/) to be informed about all unhandled exceptions.
* Use [Barryvdh\Debugbar](https://github.com/barryvdh/laravel-debugbar) during development to fast debugging.

## Storing files

* In `public` (or `public_html`) store only files that will be used by website like CSS, JS, fonts, images and other downloadable files.
* Directory listing in `public` folder should looks like this:
	* **css** - list of css files, ideally only app.css (or e.g.: website.css and admin.css)
	* **fonts** - list of fonts that are stored locally
	* **images** - list of general images used by website, used by layout, not related to dynamic data from database
	* **js** - list of JS scripts, ideally only app.js (possible also mix version with vendor.js or website.js and admin.js)
	* **uploads** - all image files that are uploaded or related to dynamic data in database
		* Two ways of handling: or just flat list of files with hashed name or grouped in subfolders for domain separation (e.g. categories, articles, avatars etc.)
* Every folder should contain empty `index.html` file to prevent directory listing.
* Only `uploads` folder should have write rights from outside group (0777) or (0755) depends on security policy.
* Remove unnecessary files when deleting related domain objects.

## Notifications

* Use notification template to send all e-mails.
* Customize template to include logotype and all static texts should be translated.
* Test sending e-mails locally with [mailtrap.io](mailtrap.io), on staging with real server SMTP account from server.

## Queues

* Use queues for time-consuming operations like e-mail sending, image compressing, populating data.
* Use redis when possible.
* Use [Laravel Horizon](https://laravel.com/docs/5.5/horizon) when possible.

## Tests

* Always write code with possibility to test it.
* Depends on system/website purpose, write particular tests:

**HTTP 200 Response** 

* Simple tests launching given URL and tests if returns 200 (no error).
* Use for assuring that no page returns error.

```php
    /**
	 * @test
	 */
    public function dashboard_200()
    {       
        $response = $this->actingAs(User::find(1))
			->get(route('dashboard'));        
        $response->assertStatus(200);
    }
```

**Feature tests**

* Should be most common type of written tests.
* Prepares data, send request and checks if system behaved accordingly.

```php
/**
 * @test
 */
public function user_can_store_a_post()
{
	$user = User::find(1);
	$this->assertTrue(0, Post::count());
	
    $this->actingAs($user)
		->post(route('post.store'), [
			'title' => 'Title post',
			'body' => 'Body post',
		]);        

	$this->assertTrue(1, Post::count());
	tap(Post::first(), function ($post) use ($user) {
		$this->assertEquals('Title post', $post->title);
		$this->assertEquals('Body post', $post->body);
		$this->assertEquals($user, $post->author);
	});
}
```

**Unit tests** 

* Tests checking correctness of parts of code.
* Use to check if given method or given functionality works properly.

```php
/**
 * @test
 */
public function is_admin_can_do_sth()
{
    $user = factory(User::class)->create(['role' => 'admin']);
    $this->assertTrue($user->canDoSth());
}
```

**Integration tests** 

* Tests validating working two or more modules or separated groups of code with each other. 
* Also use to test external services and packages.

```php
/**
 * @test
 */
public function is_external_service_working()
{
    $service = new ExternalServiceDoubler();
    $response = $service->work(2);
    $this->assert(4, $response);
}
```

**Functional tests** (aka Browser tests) 

* Automatic tests checking system behavior in browser without knowledge of internal code.
* Use to check if front-end works as desired.
* Every link/button should have unique identifier to make this tests easy.

```php
/**
 * Test if click on login button on homepage redirects to login page
 * 
 * @return void
 */
public function testLoginClick()
{
    $this->browse(function ($browser) {
        $browser->visit('/')
                ->press('#login_btn')
                ->assertPathIs('/login');
    });
}
```

## Packages

* Encapsulate universal functionalities into packages for re-use in future projects.
* If package is complete, refactored and useful - publish as an Open Source projects. But be sure to make it as good as possible - your code is yours and ours visitcard.
* During development, store packages in `App\Packages\` or use composer repository.
	* In `composer.json` add name of your package with `dev-master` version, add new repository and after `composer update` your package will be accessible inside a project:
```js
	// ..
    "repositories": [
        {
            "type": "path",
            "url": "./path/to/package",
            "options": {
                "symlink": true
            }
        }
    ],
	// ..
```

* Prepare complete `readme.MD` file and tests of package (this is also a documentation).

## Good practices

* Use `Carbon\Carbon` for date manipulation, parsing and formatting instead of `DateTime`.
