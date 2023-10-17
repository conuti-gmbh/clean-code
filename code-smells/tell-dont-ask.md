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

## Comparison with "Feature Envy"

"Tell, Don't Ask" and "[Feature Envy](feature-envy.md)" are related concepts, and they both point to issues related to the design and organization of code. While they share some similarities, they address slightly different concerns.

**Tell, Don't Ask:**
- "Tell, Don't Ask" is a design principle that encourages you to tell objects what to do rather than asking them about their state and then deciding what to do. It promotes encapsulation and a more object-oriented approach. Instead of querying an object for its state and making decisions based on that state, you should tell the object to perform an action.

**Feature Envy:**
- Feature Envy specifically refers to a situation where a method in one class seems more interested in the data of another class rather than its own. It suggests that a method is making excessive use of the methods or properties of another class, possibly indicating a better location for the method.

While addressing Feature Envy may involve moving methods closer to the data they operate on (similar to "Tell, Don't Ask"), the primary emphasis of "Tell, Don't Ask" is on the overall design philosophy, encouraging you to encapsulate behavior within objects rather than extracting data and making decisions externally.
