## !!!! Do not post your replies as comments! Clone the gist and send it to us

1. List the top 3 things you do not like about Laravel
   a) Bad use of blade, i.e I have seen people perfoming queries inside a blade view which makes the application very difficult to mantain.
   b) Sometimes perfomance for large applications is not that good.
   c) It adheres to PSR-4 coding standards; however, certain projects necessitate compliance with PSR-12. The code generated through Artisan follows PSR-4, yet the foundational code does not align with PSR-12 standards. This misalignment can lead to conflicts, particularly when employing a code sniffer in your pipeline.
3. List the top 3 things you like about Laravel
   a) Rich ecosystem
   b) Elegant syntax
   c) Very powerful ORM: Eloquent
4. For service providers, what is the difference between boot() and register()?
   The boot method is for performing actions after all service providers have been registered, the register is for binding (registering)         services into the container
6. Present a scenario (or more) where you would create ServiceProvider for your app and NOT use those already provided by Laravel
   I have created a service provider for registering a base coordenates as the main location (a store) and then given some different   coordenates calculate the distance (delivery price)
8. List some Laravel packages you have used recently and that you liked working with
   I always use the laravel debug bar as dev depency, I find it essential. Swagger for API documentation. Laravel Passport or Sanctrum for API authentication depending on the project.
10. Present some techniques to organize a Laravel application as to be loosely coupled with the framework.
    a) Use services to separate concerns between Models and Controllers.
    b) Use dependency injection to inject dependecies instead of using facades or hardcoding
    c) Use interfaces for switching implementation without affecting the rest of the application. 
12. Given the overhead they introduce why would you use Blade components instead of including sub-views?
    Because of encapsulation and reusability, however my preference is Vue. 

# Refactor this code

The purpose of the refactoring is to make the code more readable and, maybe, use features available in Laravel

Assuming we are using eloquent here and for any given reason we need to return the results as an array: 
```php
$sections = $purchase->items()
    ->whereHas('product', function ($query) {
        $query->where('type', 'electronics');
    })
    ->pluck('product.type')
    ->unique()
    ->toArray();
```

# Refactor this code
Usin php 8 new feature match
```php
function getMarketplacesStatuses($marketplaces) {
    $result = [];

    foreach ($marketplaces as &$marketplace) {
        $status = $marketplace['status'];

        $status = match ($status) {
            'active' => 'active',
            'error', 'in_progress' => 'error',
            default => 'queue',
        };

        $marketplace['status'] = $status;
        $result[$marketplace['type']] = $marketplace;
    }

    return $result;
}

```  
