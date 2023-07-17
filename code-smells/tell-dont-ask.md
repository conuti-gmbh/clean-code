# Tell, don't Ask

A good practice is not to ask for data and perform operations in the caller class, but to tell the other class what to do with the given data.

This is also the concept of [abstraction](https://stackify.com/oop-concept-abstraction/), the implementation is hidden from the caller because they don't care how exactly something works. The caller class retrieves and uses the result.

An example for a better understanding:

### Before

```php
class User
{
    private float $weight;
    private float $height;

    public function getWeight(): float
    {
        return $this->weight;
    }

    public function setWeight(float $weight): User
    {
        $this->weight = $weight;

        return $this;
    }

    public function getHeight(): float
    {
        return $this->height;
    }

    public function setHeight(float $height): User
    {
        $this->height = $height;

        return $this;
    }
}

class SomeCallerClass
{
    public function someMethod(User $user): float
    {
        // ...
        
        $bmi = round($user->getWeight() / ($user->getHeight() ** 2));
        
        // ...
    }
}
```

In this example, we need to calculate a user's BMI based on their weight and height. And we see that we get data from the object and calculate it.

A better way would be to hide the logic of the calculation in the user object and just say give me the information I need.

### After

```php
class User
{
    private float $weight;
    private float $height;
    
    public function getBMI(): float {
        return round($this->weight / ($this->height ** 2));
    }
    
    public function getWeight(): float
    {
        return $this->weight;
    }

    public function setWeight(float $weight): User
    {
        $this->weight = $weight;

        return $this;
    }

    public function getHeight(): float
    {
        return $this->height;
    }

    public function setHeight(float $height): User
    {
        $this->height = $height;

        return $this;
    }
}

class SomeCallerClass
{
    public function someMethod(User $user): float
    {
        // ...
        
        $bmi = $user->getBMI();
        
        // ...
    }
}
```

With this implementation, firstly we abstract the BMI calculation leaving only one place to change it if needed, and secondly we made it reusable for other classes.
