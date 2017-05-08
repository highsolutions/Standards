
# SQL & Eloquent

## SQL configuration

* Use MySQL (MariaDB) for a database system of the application.
* Use SQLite for testing purposes.
* Use MyISAM engine for tables with many read operations.
* Use InnoDB engine for tables with many write operations.
* Use collation for a language of the application.

## Table creation rules

* Table names should be **simple, plural, and snake_cased**.
* Tables should contain domain responsibilities as little as possible.
* **Use indexes** where suitable (primary, unique, foreign, fulltext) .
* When there is a lot of text data, you can split table into two tables: one with ids, names, dates, flags and second with text columns and columns that are not needed for search/order/filter.

## Columns creation rules

* Always use ID field with *AUTO_INCREMENT* parameter called **id**.
* Foreign keys should be named in convention: **foreign_table_id**.
* Use **title** instead of **name** as column name everywhere except tables descripting people.
* Use prefixes **is_**, **has_**, **on_** for boolean flags
* Order of columns:
	* ID
	* foreign keys
	* important columns (name, slug, etc.)
	* less important columns
	* flags (is_active, is_published)
	* timestamps (created_at, updated_at, etc.)
* Use **varchar** for every text column with max length < 400 characters
* Use **tiny_integer**, **small_integer**, **medium_integer** when suitable instead of **integer**
* Define **default** values when suitable (text columns, flags)

## Eloquent migrations

* Use Eloquent migrations to define database schema
* One file for one domain of objects:
	* blog - articles, categories, article_category
	* offers - offers, offer_applications
	* users accounts - users
* When using Laravel >=5.5 - don't implement down methods

Example:

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateCareersTables extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('careers', function(Blueprint $table) {
            $table->increments('id');
            $table->string('title', 50);
            $table->string('slug', 50);
            $table->string('photo', 100);
            $table->boolean('is_active')->default(1);
            $table->tinyInteger('seq')->unsigned();
            $table->timestamps();
			
			$table->index('title');
			$table->unique('slug');
        });

        Schema::create('career_offers', function(Blueprint $table) {
            $table->increments('id');
            $table->integer('career_id')->unsigned();
            $table->string('title', 10);
            $table->string('slug', 150);
            $table->string('salary', 20);
            $table->boolean('is_active')->default(1);
            $table->tinyInteger('seq')->unsigned();
            $table->timestamps();
			
			$table->index('title');
			$table->unique('slug');
			$table->foreign('career_id')->references('id')->on('careers');
        });

        Schema::create('career_details', function(Blueprint $table) {
            $table->increments('id');
            $table->integer('career_offer_id')->unsigned()->index();
            $table->text('description');
            $table->text('requirements');
            $table->text('additional');
            $table->text('offer');
            $table->text('salary');
            $table->string('type_of_work', 100);
            $table->string('type_of_agreement', 100);
            $table->string('benefits', 200);
            $table->string('lang', 2)->default('pl');
            $table->timestamps();
			
			$table->index('lang');
			$table->foreign('career_offer_id')->references('id')->on('career_offers');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('career_details');
        Schema::dropIfExists('career_offers');
        Schema::dropIfExists('careers');
    }
}
```

## Eloquent Seeders

* Always clear tables first.
* Try to not depend on seeders order (if you have to, write big comment about it).
* Use factories when you want to populate tables with fake data.
* If some seeding can be useful later, can be implemented as Artisan command and launched from seeder.

## Eloquent ORM

* Models should be as simple as possible, contains only necessary data and relationships.
* If there is a need for additional, repeatable responsibilities (uploading photos, sequence management, profile management, slugs etc.) move them to traits and packages and import them to Model.
* Store models in App\Models directory. In case of complex systems, you can divide models into another separate folders.
* Use **Mass assignment** of models when possible and define `$fillable` and `$guarded` attributes.
* Use **Soft deleting** when you won't delete records from database forever. Especially useful, when you have to store sensitive data or you have correlated.
* Use **Global scopes** to define quere constraints that should be used always.
* Use **Local and dynamic scopes** to define often used query constraints (is_active, order by name etc.).
* Use **Events** handling for all operations based on saving, updating, deleting events.
* Use **Eager loading** whenever you know you will need relationships of model.
	* Use constraints to only fetch required models and/or to sort them in right way.
* Use **Collections' methods** whenever possible and chain them to increase efficiency.
* Use **Accessors** when you need special calculated attributes (uppercased, concatenated, etc.) instead of method like getFullName().
* Use **Mutators** to automatically update Model attributes during update its attributes.
* **Cast attributes** values when necessary (when column in database has incompatible type with Model attribute).