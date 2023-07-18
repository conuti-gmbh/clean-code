# The "New" Keyword in Classes

Creating a class within another class creates a very tight [coupling](https://www.javatpoint.com/software-engineering-coupling-and-cohesion) between them.

It's difficult to test the classes separately because you can't change the behavior of the internal class. And if you ever need to add functionality, you'll have a hard time implementing it safely.

### Before

```php
class WebSearch
{
   private HttpClient $httpClient;
   
   public function __constructor()
   { 
       $this->httpClient = new HttpClient();
   }
}
```

### After

```php
class WebSearch
{
   public function __constructor(private HttpClient $httpClient)
   {
   }
}
```

Especially when working with Symfony, you should load classes, trough [dependency injection](https://symfony.com/doc/current/components/dependency_injection.html) and let the DI manager take care of instantiating the objects.
