# Strategy Pattern

## What is it?

The **Strategy Pattern** is a design pattern that enables dynamic alteration of an object's behavior by encapsulating interchangeable algorithms.

## Where is it Used?

* Ideal for scenarios where algorithms vary independently of their context.
* Commonly applied in areas such as algorithm selection, sorting strategies, and behavioral variations.

## Advantages

* Modularity: Encourages modular code design.
* Clean Code: Promotes clean and maintainable code architecture.
* Adaptability: Allows for dynamic switching of algorithms at runtime.

## Example

This example illustrates the Strategy Pattern in PHP, where different shipping cost calculation algorithms (strategies) are encapsulated, allowing flexible and runtime interchangeability within a shipping context.

### 1. Strategy Interface (`ShippingStrategy`)

The `ShippingStrategy` interface defines the contract that concrete shipping strategy classes must implement. In this case, it has a single method `calculateCost` that takes the product price and returns the calculated shipping cost.

```php
interface ShippingStrategy {
    public function calculateCost(float $productPrice): float;
}
```

### 2. Concrete Strategies (`StandardShipping` and `ExpressShipping`)

Two concrete strategy classes implement the `ShippingStrategy` interface. Each class provides its own implementation of the `calculateCost` method, representing different shipping cost calculation algorithms.

```php
class StandardShipping implements ShippingStrategy {
    public function calculateCost(float $productPrice): float {
        return 5 + ($productPrice * 0.1);
    }
}

class ExpressShipping implements ShippingStrategy {
    public function calculateCost(float $productPrice): float {
        return 5 + ($productPrice * 0.1) + 7;
    }
}
```

### 3. Context Class (`ShippingContext`)

The `ShippingContext` class represents the context in which the strategy is used. It has a reference to a `ShippingStrategy` and delegates the calculation of shipping cost to the strategy.

```php
readonly class ShippingContext {
    public function __construct(private ShippingStrategy $strategy) 
    {
    }

    public function calculateShippingCost(float $productPrice): float {
        return $this->strategy->calculateCost($productPrice);
    }
}
```

### 4. Client Code

In the client code:

- We create instances of the concrete strategy classes (`$standardShipping` and `$expressShipping`).
- For each strategy, we create a new instance of the `ShippingContext` and pass the corresponding strategy as an argument.
- We then use each context to calculate the shipping cost for different product prices.

```php
$productPrice1 = 50;
$productPrice2 = 100;

$standardShipping = new StandardShipping();
$context1 = new ShippingContext($standardShipping);
$cost1 = $context1->calculateShippingCost($productPrice1);
echo "Shipping cost for product 1: $cost1" . PHP_EOL;

$expressShipping = new ExpressShipping();
$context2 = new ShippingContext($expressShipping);
$cost2 = $context2->calculateShippingCost($productPrice2);
echo "Shipping cost for product 2: $cost2" . PHP_EOL;
```

This demonstrates how the Strategy Pattern allows you to encapsulate different algorithms (shipping cost calculations, in this case) and easily switch between them by creating new instances of the context with the desired strategy. 

The flexibility provided by the Strategy Pattern is especially useful when dealing with varying behaviors that can change at runtime.
