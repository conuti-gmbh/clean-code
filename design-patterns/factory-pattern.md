# Factory Pattern

## What is it?

The **Factory Pattern** is a creational design pattern widely employed in software development to encapsulate the process of creating objects. It introduces an interface for creating objects within a superclass while allowing subclasses to alter the type of objects that will be created. This enhances code flexibility, maintainability, and scalability.

## Where is it Used?

- **Object Creation Decoupling:** It is particularly useful when a system needs to be independent of its object creation, allowing for flexibility in choosing object types.

- **Dynamic Object Creation:** Ideal for scenarios where object creation involves complex configurations or when different variations of objects need to be created.

## Advantages

- **Flexibility:** Enables easy switching between different implementations or types of objects without altering the client code.

- **Maintainability:** Centralizes object creation logic, simplifying management and updates.

- **Scalability:** Supports the addition of new object types or variations without modifying existing code.

- **Decoupling:** Reduces dependencies between client code and concrete classes, promoting a modular and adaptable design.

## Example

Creating Restaurant Dishes:

First we have an example of poorly constructed code:

```php
<?php

class Dish
{
    public function __construct(private string $name, private string $ingredients)
    {
    }

    public function serve(): string
    {
        return sprintf("Serve %s, Ingredients: %s", $this->name, $this->ingredients);
    }
}

class DishCreationClient
{
    private Dish $dish;

    public function __construct(private string $name, private string $ingredients)
    {
        $this->dish = new Dish($name, $ingredients);
    }

    public function createAndServeDish(): void
    {
        echo $this->dish->serve() . PHP_EOL;
    }
}

// Client code
$clientForCapreseSalad = new DishCreationClient('Caprese Salad', 'Tomato, mozzarella, basil');
$clientForGrilledSalmon = new DishCreationClient('Grilled Salmon', 'Fresh grilled salmon with lemon');
$clientForChocolateMousse = new DishCreationClient('Chocolate Mousse', 'Rich chocolate mousse');

$clientForCapreseSalad->createAndServeDish(); // Output: Serve: Caprese Salad, Ingredients: Tomato, mozzarella, basil
$clientForGrilledSalmon->createAndServeDish(); // Output: Serve: Grilled Salmon, Ingredients: Fresh grilled salmon with lemon
$clientForChocolateMousse->createAndServeDish(); // Output: Serve: Chocolate Mousse, Ingredients: Rich chocolate mousse
```

The code above doesn't inherently have any critical issues, but it may benefit from refactoring using the factory pattern for a few reasons:

1. **Separation of Concerns:**
   The factory pattern helps in separating the responsibility of object creation from the client code. In the current code, the `DishCreationClient` class is directly responsible for creating instances of the `Dish` class. Using a factory, this responsibility is moved to a separate class.

2. **Flexibility and Extensibility:**
   With a factory pattern, it becomes easier to introduce new types of dishes or modify the creation process. If you decide to add more complex logic during the creation of a dish in the future, having a factory class provides a single point of modification.

3. **Encapsulation:**
   The factory pattern encapsulates the details of object creation. This encapsulation can hide complex creation logic, making the client code cleaner and less dependent on the internal details of the `Dish` class.

Here's how you might refactor the code using a factory pattern:

```php
<?php

interface Dish
{
    public function serve(): string;
}

class CapreseSalad implements Dish
{
    public function __construct(
        private string $name,
        private string $ingredients
    ) {}

    public function serve(): string
    {
        return sprintf("Serve %s, Ingredients: %s", $this->name, $this->ingredients);
    }
}

class GrilledSalmon implements Dish
{
    public function __construct(
        private string $name,
        private string $ingredients
    ) {}

    public function serve(): string
    {
        return sprintf("Serve %s, Ingredients: %s", $this->name, $this->ingredients);
    }
}

class ChocolateMousse implements Dish
{
    public function __construct(
        private string $name,
        private string $ingredients
    ) {}

    public function serve(): string
    {
        return sprintf("Serve %s, Ingredients: %s", $this->name, $this->ingredients);
    }
}

class DishFactory
{
    public function createDish(string $dishClass, string $name, string $ingredients): Dish
    {
        if (!class_exists($dishClass) || !in_array('Dish', class_implements($dishClass))) {
            throw new InvalidArgumentException("Invalid dish class: $dishClass");
        }

        return new $dishClass($name, $ingredients);
    }
}

class DishCreationClient
{
    public function __construct(private Dish $dish)
    {
    }

    public function createAndServeDish(): void
    {
        echo $this->dish->serve() . PHP_EOL;
    }
}

// Client code using the Factory Pattern
$factory = new DishFactory();

// Create and serve Caprese Salad
$capreseSalad = $factory->createDish(CapreseSalad::class, 'Caprese Salad', 'Tomato, mozzarella, basil');
$clientForCapreseSalad = new DishCreationClient($capreseSalad);
$clientForCapreseSalad->createAndServeDish();

// Create and serve Grilled Salmon
$grilledSalmon = $factory->createDish(GrilledSalmon::class, 'Grilled Salmon', 'Fresh grilled salmon with lemon');
$clientForGrilledSalmon = new DishCreationClient($grilledSalmon);
$clientForGrilledSalmon->createAndServeDish();

// Create and serve Chocolate Mousse
$chocolateMousse = $factory->createDish(ChocolateMousse::class, 'Chocolate Mousse', 'Rich chocolate mousse');
$clientForChocolateMousse = new DishCreationClient($chocolateMousse);
$clientForChocolateMousse->createAndServeDish();
```

The previous code did not adhere to best practices, specifically violating the **Dependency Inversion Principle**. In the original design, high-level modules, such as `DishCreationClient`, were directly dependent on concrete implementations or low-level details, like the `Dish` class, which is not a recommended practice. 

The improved version, however, aligns with best practices by ensuring that high-level modules depend on abstractions, exemplified by the `DishFactory`. This design adheres to all SOLID principles, promoting a more modular, extensible, and maintainable codebase.

```php
<?php

interface Dish
{
    public function serve(): string;
}

class CapreseSalad implements Dish
{
    public function __construct(
        private string $name,
        private string $ingredients
    ) {}

    public function serve(): string
    {
        return sprintf("Serve %s, Ingredients: %s", $this->name, $this->ingredients);
    }
}

class GrilledSalmon implements Dish
{
    public function __construct(
        private string $name,
        private string $ingredients
    ) {}

    public function serve(): string
    {
        return sprintf("Serve %s, Ingredients: %s", $this->name, $this->ingredients);
    }
}

class ChocolateMousse implements Dish
{
    public function __construct(
        private string $name,
        private string $ingredients
    ) {}

    public function serve(): string
    {
        return sprintf("Serve %s, Ingredients: %s", $this->name, $this->ingredients);
    }
}

interface DishFactory
{
    public function createDish(string $name, string $ingredients): Dish;
}

class CapreseSaladFactory implements DishFactory
{
    public function createDish(string $name, string $ingredients): Dish
    {
        return new CapreseSalad($name, $ingredients);
    }
}

class GrilledSalmonFactory implements DishFactory
{
    public function createDish(string $name, string $ingredients): Dish
    {
        return new GrilledSalmon($name, $ingredients);
    }
}

class ChocolateMousseFactory implements DishFactory
{
    public function createDish(string $name, string $ingredients): Dish
    {
        return new ChocolateMousse($name, $ingredients);
    }
}

class DishCreationClient
{
    public function __construct(private DishFactory $dishFactory) {}

    public function createAndServeDish(string $name, string $ingredients): void
    {
        $dish = $this->dishFactory->createDish($name, $ingredients);
        echo $dish->serve() . PHP_EOL;
    }
}

// Client code using the Factory Pattern
$capreseSaladFactory = new CapreseSaladFactory();
$grilledSalmonFactory = new GrilledSalmonFactory();
$chocolateMousseFactory = new ChocolateMousseFactory();

$clientForCapreseSalad = new DishCreationClient($capreseSaladFactory);
$clientForGrilledSalmon = new DishCreationClient($grilledSalmonFactory);
$clientForChocolateMousse = new DishCreationClient($chocolateMousseFactory);

$clientForCapreseSalad->createAndServeDish('Caprese Salad', 'Tomato, mozzarella, basil');
$clientForGrilledSalmon->createAndServeDish('Grilled Salmon', 'Fresh grilled salmon with lemon');
$clientForChocolateMousse->createAndServeDish('Chocolate Mousse', 'Rich chocolate mousse');

```

In this refactored version, the `DishFactory` class is responsible for creating instances of specific dishes, and the `DishCreationClient` class is now more generic and takes any dish implementing the `Dish` interface. This way, the creation logic is centralized in the factory, providing a cleaner and more modular design.

## Factory pattern or Builder pattern?

There are cases where we need to decide whether to use the Factory or Builder pattern.

### Factory Pattern Example:

Use the Factory pattern when:

1. **Object Creation is Simple:**
- The creation of objects is relatively straightforward, and there are no complex configuration steps.

2. **Common Interface for Object Creation:**
- You want to encapsulate the object creation logic and provide a common interface for **creating different types of objects**.

```php
class Product
{
    public function __construct(
        private string $name, 
        private float $price
    ) {}

    public function getName(): string
    {
        return $this->name;
    }

    public function getPrice(): float
    {
        return $this->price;
    }
}

class ProductFactory
{
    public static function createProduct(string $name, float $price): Product
    {
        return new Product($name, $price);
    }
}

// Usage
$product = ProductFactory::createProduct('Example Product', 29.99);
```

### Builder Pattern Example:

Use the Builder pattern when:

1. **Complex Object Construction:**
- The construction of objects is complex, involves multiple steps, or requires **optional and customizable features**.

2. **Fluent Interface:**
- You want to create objects with a fluent interface, allowing for a clear and readable step-by-step construction process.

```php
class Product
{
    public function __construct(
        private string $name, 
        private float $price
    ) {}

    public function getName(): string
    {
        return $this->name;
    }

    public function getPrice(): float
    {
        return $this->price;
    }
}

class ProductBuilder
{
    private string $name;
    private float $price;

    public function setName(string $name): self
    {
        $this->name = $name;
        
        return $this;
    }

    public function setPrice(float $price): self
    {
        $this->price = $price;
        
        return $this;
    }

    public function build(): Product
    {
        return new Product($this->name, $this->price);
    }
}

// Usage
$product = (new ProductBuilder())
    ->setName('Example Product')
    ->setPrice(29.99)
    ->build();
```

In summary, choose the Factory pattern when you have a simple object creation process and want a common interface. Choose the Builder pattern when the object construction is complex, involves multiple steps, or requires customization with a fluent interface.
