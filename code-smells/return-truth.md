# Return the truth

The idea behind this is not to return true or false, but to return the truth declaratively.

### Before

```php
public function isEven(int $number): bool
{
    if ($number % 2 === 0) {
        return true;
    } else {
        return false;
    }
}
```

### After

```php
public function isEven(int $number): bool
{
    return ($number % 2 === 0);
}
```
