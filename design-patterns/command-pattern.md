# Command Pattern

## What is it?

The **Command Pattern** is a behavioral design pattern that encapsulates a request as an object, thereby allowing for parameterization of clients with different requests, queuing of requests, and logging of the parameters of a request. It also allows for the support of undoable operations.

## Where is it used?

The Command Pattern is commonly used in scenarios where the sender of a request should not couple with its receiver. It's particularly useful in systems that require command queuing, undo/redo functionalities, or implementing high-level operations that are built on a series of lower-level actions.

## Advantages

- **Decouples Sender and Receiver:**
    - The sender of a request doesn't need to know anything about the receiver or its implementation details.

- **Supports Undo/Redo Operations:**
    - Commands can be easily stored and executed in reverse order, enabling undo and redo functionalities.

- **Flexibility and Extensibility:**
    - New commands can be added without altering existing client code, promoting an open/closed principle.

- **Command Queuing:**
    - Commands can be queued, providing a convenient way to implement features like job scheduling.

## Example

```php
class Light 
{
    public function turnOn(): void {
        echo "Light is on.\n";
    }

    public function turnOff(): void {
        echo "Light is off.\n";
    }
}

class LightSwitch 
{
    public function operate(string $operation): void {
        $light = new Light();
        if ($operation === 'on') {
            $light->turnOn();
        } elseif ($operation === 'off') {
            $light->turnOff();
        }
    }
}

// Usage
$switch = new LightSwitch();
$switch->operate('on');
```

The bad example demonstrates a design that lacks separation of concerns and violates some key principles of good software design. Here are some reasons why the bad example is considered bad:

1. **Lack of Separation of Concerns:**
    - The `LightSwitch` class is responsible for both creating a `Light` object and deciding what operation to perform on it (`turnOn` or `turnOff`). This violates the Single Responsibility Principle (SRP), which states that a class should have only one reason to change. In this case, the class has multiple reasons to change (e.g., if the behavior of the light changes or if a different type of light is used).

2. **Hard-Coded Dependencies:**
    - The `LightSwitch` class instantiates a `Light` object internally, creating a hard-coded dependency. This makes it difficult to extend or change the behavior of the system, as any modification requires altering the `LightSwitch` class.

3. **Limited Flexibility:**
    - Adding a new operation or command involves modifying the existing code in the `LightSwitch` class. This is not in line with the Open/Closed Principle, which suggests that classes should be open for extension but closed for modification.

4. **Poor Reusability:**
    - The lack of a clear abstraction for commands makes it challenging to reuse the code. For instance, if you want to use the same `Light` object in a different context or with a different set of commands, you would need to duplicate and modify the code.

5. **No Support for Undo/Redo or Queuing:**
    - The design does not naturally support undo/redo operations or command queuing. These features often become necessary in real-world applications and are more challenging to implement without a well-defined command pattern.

6. **Testing Challenges:**
    - Testing the `LightSwitch` class is difficult because it directly creates `Light` objects. In a more modular and testable design, dependencies should be injected, making it easier to replace them with mock objects during testing.

In summary, the bad example lacks modularity, flexibility, and adherence to key design principles. 

The Command Pattern, as demonstrated in the refactored example, addresses these issues by introducing a clear separation of concerns, encapsulating commands, and promoting a more extensible and maintainable design.

*Refactored Code Using Command Pattern:*

```php
class Light 
{
    public function turnOn(): void {
        echo "Light is on.\n";
    }

    public function turnOff(): void {
        echo "Light is off.\n";
    }
}

interface Command 
{
    public function execute();
}

class LightOnCommand implements Command 
{
    public function __construct(private Light $light) 
    {
    }

    public function execute() {
        $this->light->turnOn();
    }
}

class LightOffCommand implements Command 
{
    public function __construct(private Light $light) 
    {
    }

    public function execute() {
        $this->light->turnOff();
    }
}

class RemoteControl 
{
    public function setCommand(private Command $command) 
    {
    }

    public function pressButton() {
        $this->command->execute();
    }
}

// Usage
$light = new Light();
$lightOn = new LightOnCommand($light);
$lightOff = new LightOffCommand($light);

$remote = new RemoteControl();

$remote->setCommand($lightOn);
$remote->pressButton(); // Light is on.

$remote->setCommand($lightOff);
$remote->pressButton(); // Light is off.
```

In the refactored code, the Command Pattern is applied to decouple the sender (`RemoteControl`) from the receiver (`Light`). New commands can be added without modifying existing classes, promoting flexibility and extensibility. Additionally, this structure allows for the implementation of undo/redo features or command queuing if needed.
