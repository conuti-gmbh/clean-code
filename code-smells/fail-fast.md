# Fail Fast / Early Exit 

The idea behind this is to trigger terminations at the beginning of each function and continue on the happy path after that.

Using early exits keeps your code visually flatter and cleaner.

### Before

```php
public function getCountryName(string $countryCode): string
{
    $countries = [
        'de' => 'Germany',
        'fr' => 'France',
        'ar' => 'Argentina',
    ];

    if (strlen($countryCode) === 2) {
        if (array_key_exists($countryCode, $countries)) {
            return $countries[$countryCode];
        } else {
            throw new InvalidArgumentException("Country code not exist");
        }
    } else {
        throw new InvalidArgumentException("Country code not valid");
    }
}
```

### After

```php
public function getCountryName(string $countryCode): string
{
    $countries = [
        'de' => 'Germany',
        'fr' => 'France',
        'ar' => 'Argentina',
    ];

    if (strlen($countryCode) !== 2) {
        throw new InvalidArgumentException("Country code not valid");
    }
    
    if (!array_key_exists($countryCode, $countries)) {
        throw new InvalidArgumentException("Country code not exist");
    }

    return $countries[$countryCode];
}
```

You can read more about this topic from [Martin Fowler](https://www.martinfowler.com/ieeeSoftware/failFast.pdf).
