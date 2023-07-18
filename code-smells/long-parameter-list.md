# Long Parameter List

The idea behind this is that if we have a function with a long list of parameters, we should consider how to group them and create new classes from them and pass the objects as parameters.

There is no single rule for how many is too many parameters. Usually, more than three or four is considered too many.

### Before

```php
class User
{
    public function __construct(
        private string $username,
        private string $password,
        private string $state,
        private string $city,
        private string $street,
        private string $houseNumber,
    ) {}
}
```

After checking the parameters, we can simply extract the address part into a new class (e.g [Value Object](https://enterprisecraftsmanship.com/posts/value-objects-explained/)) and pass the object as a parameter.

### After

```php
class AddressVO
{
    public function __construct(
        private string $state,
        private string $city,
        private string $street,
        private string $houseNumber,
    ) {}
}

class User
{
    public function __construct(
        private string $username,
        private string $password,
        private AddressVO $address,
    ) {}
}
```

That's it! The parameter list now looks cleaner. The code is more readable and shorter. Also, may reveal previously unnoticed duplicate code.
