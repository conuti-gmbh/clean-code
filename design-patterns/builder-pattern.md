# Builder Pattern

## What is it?

The Builder Pattern is a creational design pattern that facilitates the construction of complex objects by separating the construction process from the actual representation. It provides a **step-by-step approach to building an object**, allowing the construction of various representations with the same construction process.

## Where is it Used?

- **Complex Object Construction:** The Builder Pattern is particularly useful when creating objects with many configuration options or complex initialization steps.

- **Variability in Object Configuration:** It is employed when an object's configuration can result in different representations, and there's a need for flexibility without modifying the client code.

## Advantages:

- **Flexible Object Construction:**
    - Allows the construction of diverse objects using the same building process.

- **Readability and Maintainability:**
    - Enhances code readability by separating the construction steps.
    - Improves maintainability, as each building step is encapsulated within its own method.

- **Configurability:**
    - Provides a convenient way to configure objects with numerous optional parameters.

## Example:

Consider the example of constructing a `Computer` object in PHP.

First we have an example of poorly constructed code:

```php
<?php

class Computer 
{
    public $processor;
    public $ram;
    public $hardDrive;

    public function display(): void {
        echo "Processor: {$this->processor}, RAM: {$this->ram}, Hard Drive: {$this->hardDrive}\n";
    }
}

class ComputerManufacturer 
{
    public function createGamingComputer(): Computer {
        $computer = new Computer();
        $computer->processor = "Intel Core i7";
        $computer->ram = "16 GB";
        $computer->hardDrive = "1 TB SSD";
        
        return $computer;
    }

    public function createOfficeComputer(): Computer {
        $computer = new Computer();
        $computer->processor = "Intel Core i5";
        $computer->ram = "8 GB";
        $computer->hardDrive = "500 GB HDD";
        
        return $computer;
    }
}

// Client code
$manufacturer = new ComputerManufacturer();

$gamingComputer = $manufacturer->createGamingComputer();
echo "Gaming Computer created: ";
$gamingComputer->display();

$officeComputer = $manufacturer->createOfficeComputer();
echo "Office Computer created: ";
$officeComputer->display();
```

The code above has several issues:

1. **Lack of Encapsulation:**
  - The properties (`$processor`, `$ram`, `$hardDrive`) of the `Computer` class are public, violating the principle of encapsulation. Direct access to properties can lead to unintended modifications and hinder future maintenance.

2. **Limited Flexibility and Configurability:**
  - The `ComputerManufacturer` class directly assigns values to the `Computer` properties during construction, limiting the flexibility to create computers with different configurations. This approach is not scalable for handling variations in computer specifications.

3. **Poor Separation of Concerns:**
  - The construction and configuration logic is tightly coupled within the `ComputerManufacturer` class, leading to low cohesion and poor separation of concerns. This makes it challenging to extend or modify the code without affecting other parts of the system.

4. **Maintenance Challenges:**
  - Any changes in the structure of the `Computer` class or the configuration logic in the `ComputerManufacturer` class may require modifications throughout the codebase, increasing the risk of errors and making maintenance cumbersome.

To address these issues, adopting a design pattern like the Builder Pattern, as demonstrated in the improved example, can enhance code organization, maintainability, and configurability.

```php
class Computer 
{
    // ... (private properties)

    public function display(): void {
        // Display the computer details
    }
}

class ComputerBuilder 
{
    private string $processor = "Default Processor";
    private string $ram = "Default RAM";
    private string $hardDrive = "Default Hard Drive";

    // Set the processor
    public function setProcessor(string $processor): self {
        $this->processor = $processor;
        
        return $this;
    }

    // Set the RAM
    public function setRam(string $ram): self {
        $this->ram = $ram;
        
        return $this;
    }

    // Set the hard drive
    public function setHardDrive(string $hardDrive): self {
        $this->hardDrive = $hardDrive;
        
        return $this;
    }

    // Build the computer with the configured options
    public function build(): Computer {
        $computer = new Computer();
        $computer->setProcessor($this->processor);
        $computer->setRam($this->ram);
        $computer->setHardDrive($this->hardDrive);
        
        return $computer;
    }
}

class ComputerManufacturer 
{
    public function __construct(private ComputerBuilder $computerBuilder) 
    {
    }

    public function createGamingComputer(): Computer {
        return $this->computerBuilder
            ->setProcessor("Intel Core i7")
            ->setRam("16 GB")
            ->setHardDrive("1 TB SSD")
            ->build();
    }

    public function createOfficeComputer(): Computer {
        return $this->computerBuilder
            ->setProcessor("Intel Core i5")
            ->setRam("8 GB")
            ->setHardDrive("500 GB HDD")
            ->build();
    }
}

// Client code
$computerBuilder = new ComputerBuilder();
$manufacturer = new ComputerManufacturer($computerBuilder);

$gamingComputer = $manufacturer->createGamingComputer();
echo "Gaming Computer created: ";
$gamingComputer->display();

$officeComputer = $manufacturer->createOfficeComputer();
echo "Office Computer created: ";
$officeComputer->display();
```

This example demonstrates the Builder Pattern's application in constructing Computer objects with varying configurations, enhancing flexibility, readability, and maintainability in the codebase.
