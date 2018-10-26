## Testing service providers

Here we verify that when we resolve an interface, we get an item of the correct class back:

```php
<?php

namespace Tests\Unit\Providers;

use Mockery as m;
use Tests\TestCase;

class ServiceProviderTest extends TestCase
{
    /** @test */
    public function it_sets_up_the_comments_repository()
    {
        $repo = $this->app->make('Matthewbdaly\LaravelComments\Contracts\Repositories\Comment');
        $this->assertInstanceOf(\Matthewbdaly\LaravelComments\Eloquent\Repositories\Decorators\Comment::class, $repo);
    }

    /** @test */
    public function it_sets_up_the_flag_repository()
    {
        $repo = $this->app->make('Matthewbdaly\LaravelComments\Contracts\Repositories\Comment\Flag');
        $this->assertInstanceOf(\Matthewbdaly\LaravelComments\Eloquent\Repositories\Decorators\Comment\Flag::class, $repo);
    }
}
```

Here, we have a facade for a shopping cart. We mock out our application's cart service using Mockery, and set an assertion that it should receive a call to the `destroy()` method. Then, we call it from the facade. That way, we're able to confirm that calling the methods via the facade passes those method calls along to an instance of the underlying class:

```php
<?php

namespace Tests\Unit\Providers;

use Tests\TestCase;
use Mockery as m;
use Cart;

class ServiceProviderTest extends TestCase
{
    public function testFacade()
    {
        $mock = m::mock('Matthewbdaly\LaravelCart\Contracts\Services\Cart');
        $mock->shouldReceive('destroy')->once();
        $this->app->instance('Matthewbdaly\LaravelCart\Contracts\Services\Cart', $mock);
        $this->assertInstanceOf('Matthewbdaly\LaravelCart\Contracts\Services\Cart', $this->app['Matthewbdaly\LaravelCart\Contracts\Services\Cart']);
        Cart::destroy();
    }
}
```
