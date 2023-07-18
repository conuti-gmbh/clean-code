# Primitive Obsession

Primitive obsession is a code smell in which primitive data types are used excessively to represent your data models.

Like most other smells, primitive obsessions are born in moments of weakness. "Just a field for storing some data!" the programmer said.

### Before

```php
class CheckingAccount {
   private int $accountNumber;
   private string $customerName;
   private string $email;
   private string $street;
   private string $houseNumber;
   private int $zipCode;
   private string $city;
   private string $state;
   private string $country;
   private string $socialSecurityNumber;
   private DateTime $activeDate;
}
```

After checking the properties, we can simply extract different pieces that belong together into different new classes (e.g. [Value Object](https://enterprisecraftsmanship.com/posts/value-objects-explained/)) and use the class instead of the primitive types.

### After

```php
class Address {
   private string $street;
   private string $houseNumber;
   private int $zipCode;
   private string $city;
   private string $state;
   private string $country;
}

class SocialSecurity {
   private string $Ssn;
}

class CheckingAccount {
   private int $accountNumber;
   private string $customerName;
   private string $email;
   private Address $address;
   private SocialSecurity $socialSecurity;
   private DateTime $activeDate;
}
```
