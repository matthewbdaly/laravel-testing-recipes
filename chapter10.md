## Testing pretty much anything else

Now that we've got a good handle on using Mockery, then testing virtually anything else within our Laravel application should be a piece of cake, **provided we've structured our application correctly**.

If we're using dependency injection throughout our application, then in principle we should be able to mock any dependency required by any one class. That means we can produce a test like this:

```php
<?php

namespace Tests\Services;

use Mockery as m;
use Tests\TestCase;
use App\Services\Invoicer;

class InvoicerTest extends TestCase
{
    public function tearDown()
    {
        m::close();
        parent::tearDown();
    }

    public function testCreateInvoice()
    {
        $repository = m::mock('App\Repositories\Order');
        $repository->shouldReceive('allUnpaid')->once()->andReturn([
            'id' => 1,
            'name' => 'Smethwick Systems',
        ]):
        $invoicer = new Invoicer($repository);
        $response = $invoicer->invoice();
        $this->assertCount(1, $response);
        $this->assertEquals(1, $response[0]->id);
        $this->assertEquals('Smethwick Systems', $response[0]->name);
    }
}
```

Here we're testing an `Invoicer` class that accepts an `Order` repository as its sole dependency in the constructor. We replace that dependency with a mock that expects the `allUnpaid()` method to be called and specifies the return value. Then we assert that the response from the invoicer is what we expect.
