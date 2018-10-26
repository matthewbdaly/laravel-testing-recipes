## Testing models

The strategy I use for testing Laravel models is based on the one I learned for testing Django models - like Laravel, Django has an Active Record-style ORM, so similar strategies can be used.

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

$factory->define(Tripstream\Eloquent\Models\User::class, function (Faker $faker) {
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

$factory->define(Tripstream\Eloquent\Models\Trip::class, function (Faker $faker) {
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

$factory->define(Tripstream\Eloquent\Models\Post::class, function (Faker $faker) {
    return [
        'title' => $faker->word,
        'description' => $faker->sentence,
    ];
});
```
