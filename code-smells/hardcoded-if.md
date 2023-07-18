# Hardcoded If

Hard-coding is good for prototyping and learning, but then we **need to generalize and refactor our solutions**.

### Before

```php
public function getCountryName(string $countryCode): string
{
    if ($countryCode === "de") {
        return "Germany";
    } elseif ($countryCode === "fr") {
        return "France";
    } elseif ($countryCode === "ar") {
        return "Argentina";
    } // lots of elseif
    else {
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
    
    if(!array_key_exists($countryCode, $countries)) {
        throw new InvalidArgumentException("Country code not valid");
    }
    
    return $countries[$countryCode];
}
```
