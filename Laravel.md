# Laravel

Follow Style Guide for [PHP](/highsolutions/Standards/wiki/PHP).

## Installation

* Keep in mind server requirements and choose the newest version of Laravel that fulfills PHP version on production server.
* Configure .env.server file to be ready to just rename it to .env on server.
* Configure GitHub hook to Continuous Integration on every push.
* Use *.dev or *.local developer domain on your local machine.
* Do not store `.env` file, `composer.lock` file, `storage` folder, and `vendor` folder in repo.

## Configuration

* Use `.env` files for every setting of application that can vary between developers computers, staging server and production server.
* Use config files to define every setting for application that is constant. Use .env variables when necessary.
* Do not store API keys etc. inside code. Put them into .env or config files.
* Dynamic settings store in database or in translation files (if depend on localization).

## Controllers

* Organize controllers in subfolders when application consists of more than one module (e.g. Website, Admin, Auth).
* Use [Resource Controllers](https://laravel.com/docs/controllers#resource-controllers) when possible and method names from this convention (index, show, create, store, edit, update, destroy). 
* Controllers for admin panel create using `Route::resource`. Use parameters `only` and `except` when necessary.
* Methods should have only few responsibilities:
  * Displaying data method
    * Fetch data from repositories/services
    * Display view with required data

Example:

```php
/**
 * Display list of posts of category
 *
 * @return Response
 */
public function show_category($category_id)
{
    $posts = $this->repo->getByCategory($category_id);

    return view('blog/posts', [
        'posts' => $posts
    ]);
}
```

  * Receiving data method
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
    $this->validate($request, [
        'title' => 'required|unique:posts|max:255',
        'body' => 'required',
    ]);

    $status = $this->repo->store(request());

    return response()->json(['status' => $status]);
}   
``` 
 
  * With more advanced request validation, [FormRequest](https://laravel.com/docs/5.4/validation#form-request-validation) should be used.
  * When method should act differently based on parameters/input data, use private methods to keep method clean and short.
  * When controller has to send many static data to view (not based on parameters), use [ViewComposer](https://laravel.com/docs/views#view-composers).

## Services, Traits and Packages

* Extend logic of your code in three ways:
  * **Services** - when you need simple functionality from the black-box
  * **Traits** - when you want to extend Model/Controller functionality
  * **Packages** - when you need full module (from vendor or your own)
* Use Service Providers 

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

## Commands

* Write commands for operations which are launched only by developers in console.
* Write descriptions to know what command do.
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
* Install [Supervisor](https://laravel.com/docs/5.4/queues#supervisor-configuration) when possible.

## Tests

* Always write code with possibility to test it.
* Depends on system/website purpose, write particular tests:

**HTTP 200 Response** 

* Simple tests launching given URL and tests if returns 200 (no error).
* Use for assuring that no page returns error.

```php
    /**
     * Check if dasboard return 200 HTTP Response
     *
     * @return void
     */
    public function testDashboard()
    {       
        $response = $this->actingAs(User::find(1))
			->get(route('dashboard'));        
        $response->assertStatus(200);
    }
```

**Feature tests** 

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
* If package is complete, refactored and useful - publish as Open Source projects. But be sure to make it as good as possible - your code is yours and ours visitcard.
* During development, store packages in `App\Packages\`.
* Prepare complete `readme.MD` file and tests of package (this is also a documentation).