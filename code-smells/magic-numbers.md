# Magic Numbers

Magic numbers refer to the numbers used directly in the code rather than storing them in some meaningful variable. Using magic numbers is considered a bad practice in programming as they can cause ambiguity and confusion.

### Before

```php
class SomeClass
{
    public function energy($mass)
    {
        return $mass * 299792458 ** 2;
    }
}
```

### After

```php
class SomeClass
{
    public const LIGHT_SPEED = 299792458;

    public function energy($mass)
    {
        return $mass * self::LIGHT_SPEED ** 2;
    }
}
```
