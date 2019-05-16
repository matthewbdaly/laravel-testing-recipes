## Testing models

The strategy I use for testing Laravel models is based on the one I learned for testing Django models - like Laravel, Django has an Active Record-style ORM, so similar strategies can be used.

It's arguable as to whether ORM models themselves actually need much in the way of testing if you don't add any custom functionality. There's no point in wasting time and effort testing the basic functionality of a model, since that's provided by Eloquent, which has its own tests. For that reason, my tests tend to focus on the following:

* That we can create an instance of the required model
* That we can retrieve it
* That we can retrieve the fields correctly

This tests the following:

* That the model class exists
* That the migration(s) to create and update the model class have set up the correct fields

Since Django defines the fields in the model classes, in that context this approach would just test the models. In the context of a Laravel application, this approach actually tests both the migrations and the models, which makes sense as the migrations are the definitive source of truth in terms of the database structure, and so we ought to test them to make sure they are creating the right structure.

Let's see our first model test:

```php
<?php

namespace Tests\Unit\Eloquent\Models;

use Tests\TestCase;
use Illuminate\Foundation\Testing\WithFaker;
use Illuminate\Foundation\Testing\RefreshDatabase;
use App\Trip;
use App\User;
use App\Post;

class PostTest extends TestCase
{
    use RefreshDatabase;

    /**
     * Test create post
     *
     * @return void
     */
    public function testCreatePost()
    {
        $user = factory(User::class)->create([]);
        $trip = factory(Trip::class)->create([
            'user_id' => $user->id,
        ]);
        factory(Post::class)->create([
            'title' => 'Dinner on Tuesday',
            'description' => 'This is my dinner on Tuesday',
            'trip_id' => $trip->id,
        ]);
        $post = Post::first();
        $this->assertEquals('Dinner on Tuesday', $post->title);
        $this->assertEquals('This is my dinner on Tuesday', $post->description);
        $this->assertEquals($trip->id, $post->trip->id);
    }
}
```

Here we have a fairly typical example of a simple model test I might write for the `Post` class in an application for blogging about journeys. Each journey, denoted by the `Trip` model, has a user, denoted by the `User` model, and a trip can have many posts.

Our first step is to create a user, since the other models depend on it:

```php
$user = factory(User::class)->create([]);
```

Note the use of the factory function to create a user. This is defined in `database/factories/UserFactory.php`:

```php
<?php

use Faker\Generator as Faker;

/*
|--------------------------------------------------------------------------
| Model Factories
|--------------------------------------------------------------------------
|
| This directory should contain each of the model factory definitions for
| your application. Factories provide a convenient way to generate new
| model instances for testing / seeding your application's database.
|
*/

$factory->define(App\User::class, function (Faker $faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->unique()->safeEmail,
        'password' => '$2y$10$TKh8H1.PfQx37YgCzwiKb.KjNyWgaHb9cbcoQgdIVFlYg7B77UdFm', // secret
        'remember_token' => str_random(10),
    ];
});
```

Next, we create our trip:

```php
        $trip = factory(Trip::class)->create([
            'user_id' => $user->id,
        ]);
```

Again, we're using a factory to create the required fixture. This is defined in `database/factories/TripFactory.php`:

```php
<?php

use Faker\Generator as Faker;
use Carbon\Carbon;

$factory->define(App\Trip::class, function (Faker $faker) {
    return [
        'title' => $faker->word,
        'description' => $faker->sentence,
        'start_date' => Carbon::now(),
        'end_date' => Carbon::now(),
        //
    ];
});
```

Next step is to create the post model, again using a factory:

```php
        factory(Post::class)->create([
            'title' => 'Dinner on Tuesday',
            'description' => 'This is my dinner on Tuesday',
            'trip_id' => $trip->id,
        ]);
```

Here's that factory:

```php
<?php

use Faker\Generator as Faker;

$factory->define(App\Post::class, function (Faker $faker) {
    return [
        'title' => $faker->word,
        'description' => $faker->sentence,
    ];
});
```

Using an in-memory SQLite database
==================================

It's vitally import that your test suite be as fast as possible, at least for unit tests. Slow test suites disrupt your workflow. Your entire suite of unit tests (and ideally integration tests too), should all run within ten seconds if at all possible, because any longer and your mind will start to wander.

It's therefore a good idea to use an in-memory SQLite database if at all possible. It ensures that any tests that interact with the database will be lightning-fast, and helps keep your test suite inside the magic 10-second limit. To do so, make sure the `DB_CONNECTION` and `DB_DATABASE` keys are set as follows in your `phpunit.xml`:

```xml
        <env name="APP_ENV" value="testing"/>
        <env name="CACHE_DRIVER" value="array"/>
        <env name="DB_CONNECTION" value="sqlite"/>
        <env name="DB_DATABASE" value=":memory:"/>
```

You should also use the `RefreshDatabase` trait in any tests that interact with the database, as that will automatically destroy the database after each test, then re-migrate it for the next one.

A word of warning - using SQLite for your tests depends on you not using any database-specific functionality or raw queries. If you call anything that's specific to MySQL or PostgreSQL, or use a raw query, you can almost certainly no longer rely on your tests being consistent between different databases, and should switch to testing against your production database. However, as long as you stick to the functionality explicitly provided by Eloquent, and never use methods such as `raw`, `whereRaw`, or `selectRaw`, you should be fine.
