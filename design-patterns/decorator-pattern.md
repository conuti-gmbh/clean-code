# Decorator Pattern

## What is it?

The **Decorator pattern** is a structural design pattern that allows behavior to be added to an object, either statically or dynamically, without affecting the behavior of other objects from the same class. It is a flexible alternative to subclassing for extending functionality.

## Where is it used?

The Decorator pattern is commonly used in scenarios where:

- **Dynamic Extension:** You want to add or alter the behavior of objects at runtime.

- **Closed for Modification, Open for Extension:** You aim to extend the functionalities of classes without modifying their source code.

- **Component Composition:** You want to build a complex object by combining simpler components.

## Advantages

- **Flexibility and Extensibility:** Easily add or remove responsibilities from objects without modifying their code.

- **Open-Closed Principle:** Classes are open for extension but closed for modification, promoting a design that is easy to maintain and extend.

- **Component Composition:** Encourages a composition-based approach, allowing for the creation of complex objects through the combination of simpler ones.

## Example

*Bad Code Example (Without Decorator Pattern):*

```php
class Coffee 
{
    public function cost() {
        return 5; // Base cost of a coffee
    }
}

class MilkCoffee extends Coffee 
{
    public function cost() {
        return parent::cost() + 2; // Additional cost for milk
    }
}

class SugarMilkCoffee extends MilkCoffee 
{
    public function cost() {
        return parent::cost() + 1; // Additional cost for sugar
    }
}

// Client code
$coffee = new SugarMilkCoffee();
echo "Total cost: $" . $coffee->cost(); // Outputs: Total cost: $8
```

*Refactored Good Code (Using Decorator Pattern):*

```php
interface Coffee
{
    public function cost();
}

class SimpleCoffee implements Coffee 
{
    public function cost() {
        return 5; // Base cost of a coffee
    }
}

class MilkDecorator implements Coffee 
{
    public function __construct(private Coffee $coffee) 
    {
    }

    public function cost() {
        return $this->coffee->cost() + 2; // Additional cost for milk
    }
}

class SugarDecorator implements Coffee 
{
    public function __construct(private Coffee $coffee) 
    {
    }

    public function cost() {
        return $this->coffee->cost() + 1; // Additional cost for sugar
    }
}

// Client code
$coffee = new SugarDecorator(new MilkDecorator(new SimpleCoffee()));
echo "Total cost: $" . $coffee->cost(); // Outputs: Total cost: $8
```

In this refactored code, the Decorator pattern is used to compose different combinations of coffee and additives, providing a more flexible and maintainable solution.
