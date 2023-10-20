# Abstract Factory Pattern

## What is it?

The **Abstract Factory Pattern** is a design pattern used in object-oriented programming. It falls under the category of creational patterns and provides an interface for creating families of related or dependent objects without specifying their concrete classes.

## Where is it Used?

The Abstract Factory Pattern is employed in scenarios where a system needs to be independent of how its objects are created, composed, and represented. It is especially useful in situations where multiple families of related products exist, and the system needs to be configured with one of these families.

## Advantages:

- **Abstraction of Object Creation:**
    - Separates the client code from the actual creation of objects, promoting a high level of abstraction.

- **Consistent Product Families:**
    - Ensures that the created objects belong to the same family, providing consistency in usage.

- **Easy Replacement:**
    - Allows easy substitution of entire families of products without modifying client code.

- **Enhanced Flexibility:**
    - Facilitates the addition of new product families without altering existing client code.

## Example

In the context of PHP, consider the example of creating families of modern and Victorian furniture, encompassing chairs and tables using the Abstract Factory Pattern.

First we have an example of poorly constructed code:

```php
// Interfaces for abstract products
interface Chair 
{
    public function sitOn(): void;
}

interface Table 
{
    public function use(): void;
}

// Concrete implementations of abstract products
class ModernChair implements Chair 
{
    public function sitOn(): void {
        echo "Sitting on a modern chair\n";
    }
}

class ModernTable implements Table 
{
    public function use(): void {
        echo "Using a modern table\n";
    }
}

class VictorianChair implements Chair 
{
    public function sitOn(): void {
        echo "Sitting on a Victorian chair\n";
    }
}

class VictorianTable implements Table 
{
    public function use(): void {
        echo "Using a Victorian table\n";
    }
}

// Client code with interfaces and concrete product creation
class BadClient 
{
    public function createModernChair(): Chair {
        return new ModernChair();
    }

    public function createModernTable(): Table {
        return new ModernTable();
    }
    
    public function createVictorianChair(): Chair {
        return new VictorianChair();
    }

    public function createVictorianTable(): Table {
        return new VictorianTable();
    }

    // Client code then uses these methods to create objects
    public function useModernFurniture(): void {
        $modernChair = $this->createModernChair();
        $modernTable = $this->createModernTable();

        $modernChair->sitOn();
        $modernTable->use();
    }
    
    // Client code then uses these methods to create objects
    public function useVictorianFurniture(): void {
        $victorianChair = $this->createVictorianChair();
        $victorianTable = $this->createVictorianTable();
        
        $victorianChair->sitOn();
        $victorianTable->use();
    }
}

// Client instantiation and usage
$badClient = new BadClient();
$badClient->useModernFurniture();
$badClient->useVictorianFurniture();
```

**Why It's Considered Bad:**
1. **Lack of Abstraction:**
    - The client code is directly responsible for creating instances of concrete products (e.g., `ModernChair` and `ModernTable`). This lacks abstraction and makes the code tightly coupled to specific implementations.

2. **Violation of Single Responsibility Principle:**
    - The client class is burdened with the responsibility of both creating objects and using them. This violates the Single Responsibility Principle, which suggests that a class should have only one reason to change.

3. **Difficulty in Replacement:**
    - If there is a need to switch from modern to Victorian furniture (or vice versa), the client code has to be modified extensively, leading to maintenance challenges.

4. **Reduced Flexibility:**
    - The code is less flexible in accommodating changes. Adding a new type of furniture would require further modification of the client class, leading to code that is harder to maintain and extend.

In summary, the code violates key design principles like abstraction, separation of concerns, and flexibility, making it less maintainable and adaptable to changes. 

This is where the Abstract Factory Pattern comes into play to address these issues.

```php
// Interfaces for abstract products
interface Chair 
{
    public function sitOn(): void;
}

interface Table 
{
    public function use(): void;
}

// Concrete implementations of abstract products
class ModernChair implements Chair 
{
    public function sitOn(): void {
        echo "Sitting on a modern chair\n";
    }
}

class ModernTable implements Table 
{
    public function use(): void {
        echo "Using a modern table\n";
    }
}

class VictorianChair implements Chair 
{
    public function sitOn(): void {
        echo "Sitting on a Victorian chair\n";
    }
}

class VictorianTable implements Table 
{
    public function use(): void {
        echo "Using a Victorian table\n";
    }
}

// Abstract Factory interface
interface FurnitureFactory 
{
    public function createChair(): Chair;
    public function createTable(): Table;
}

// Concrete implementations of abstract factory
class ModernFurnitureFactory implements FurnitureFactory 
{
    public function createChair(): Chair {
        return new ModernChair();
    }

    public function createTable(): Table {
        return new ModernTable();
    }
}

class VictorianFurnitureFactory implements FurnitureFactory 
{
    public function createChair(): Chair {
        return new VictorianChair();
    }

    public function createTable(): Table {
        return new VictorianTable();
    }
}

// Client code using the Abstract Factory Pattern
class GoodClient 
{
    public function __construct(private FurnitureFactory $factory) 
    {
    }

    public function useFurniture(): void {
        $chair = $this->factory->createChair();
        $table = $this->factory->createTable();

        $chair->sitOn();
        $table->use();
    }
}

// Client instantiation and usage with a specific factory
$modernFactory = new ModernFurnitureFactory();
$goodClient = new GoodClient($modernFactory);
$goodClient->useFurniture();

$victorianFactory = new VictorianFurnitureFactory();
$goodClient = new GoodClient($victorianFactory);
$goodClient->useFurniture();
```

The refactored code using the Abstract Factory Pattern has several advantages, which can be summarized in the following bullet points:

- **Abstraction:**
    - The use of interfaces for abstract products (`Chair` and `Table`) and the abstract factory (`FurnitureFactory`) promotes a high level of abstraction. The client code interacts with these interfaces, remaining unaware of the specific product implementations.

- **Separation of Concerns:**
    - The responsibility for creating objects is separated into concrete factory classes (`ModernFurnitureFactory` and `VictorianFurnitureFactory`). The client code (`GoodClient`) focuses on using the products rather than creating them, adhering to the Single Responsibility Principle.

- **Flexibility and Extensibility:**
    - The client code can easily switch between different families of products (e.g., modern and Victorian) by providing the appropriate factory. This enhances flexibility and makes it straightforward to introduce new families of products without modifying existing code.

- **Reduced Code Duplication:**
    - The use of interfaces and abstract classes helps to reduce code duplication. Similar product creation logic is encapsulated within the factory classes, promoting a more maintainable and scalable codebase.

- **Easy Maintenance:**
    - Changes in product families or the addition of new products can be accommodated without modifying the client code. This makes the codebase easier to maintain, and modifications are localized to the respective factory classes.

- **Improved Readability:**
    - The code is more readable and follows industry best practices. The use of well-defined interfaces and adherence to design principles make it easier for other developers to understand, contribute to, and extend the code.

- **Reduced Dependency:**
    - The client code depends only on the abstract factory interface and product interfaces, reducing dependency on concrete implementations. This makes the code more resilient to changes in the underlying implementations.

In summary, the refactored code exhibits good design principles, promoting abstraction, separation of concerns, flexibility, and ease of maintenance. It provides a clean and extensible structure, making it a solid foundation for scalable and maintainable software.
