# Composite Pattern

## What is it?

The **Composite Pattern** is a structural design pattern that enables clients to treat individual objects and compositions of objects uniformly. It allows you to compose objects into tree structures to represent part-whole hierarchies. Clients can then work with individual objects or compositions without needing to distinguish between them.

## Where is it used?

The Composite Pattern is commonly used in scenarios where you have a hierarchy of objects and you want clients to treat individual objects and compositions uniformly. It is especially useful when dealing with tree structures, such as representing graphic shapes, document structures, or organizational hierarchies.

## Advantages

- **Uniformity:** Treat individual objects and compositions uniformly.
- **Simplified Client Code:** Clients can work with complex hierarchies without having to understand their internal structure.
- **Scalability:** Easy to add new components without modifying existing client code.
- **Consistency:** Promotes a consistent interface for both leaf and composite objects.

## Example

```php
class BadTask 
{
    public function __construct(private string $name) 
    {
    }

    public function execute(): void {
        echo "Executing task: {$this->name}\n";
    }
}

// Client code
$task1 = new BadTask("Task 1");
$task2 = new BadTask("Task 2");

// This breaks when you want to treat tasks as a group
$task1->execute();
$task2->execute();
```

The bad code example violates several principles of good software design, and here are some of the issues:

1. **Lack of Abstraction:**
    - In the bad code example, there's no abstraction or common interface for tasks. Each task is treated as a concrete class without a shared interface.

2. **Inflexibility:**
    - The code does not easily support the execution of a group of tasks. If you want to execute a set of tasks as a single unit, the code lacks a structure to accommodate this.

3. **Limited Scalability:**
    - Adding new tasks or modifying the execution logic would require changes to the client code, making the system less scalable and more prone to errors.

4. **No Composition:**
    - There's no concept of composing tasks into a larger structure. The client code has to explicitly manage each task individually, leading to inflexible code.

In summary, the bad code example lacks abstraction, flexibility, and scalability. It does not adhere to fundamental principles of object-oriented design, making it harder to maintain and extend. 

The Composite Pattern addresses these issues by introducing a common interface, allowing for composition, and promoting a more flexible and scalable design.

**Refactored code using Composite Pattern**

```php
interface Task 
{
    public function execute();
}

class CompositeTask implements Task 
{
    private array $tasks = [];

    public function addTask(Task $task): void {
        $this->tasks[] = $task;
    }

    public function execute(): void {
        foreach ($this->tasks as $task) {
            $task->execute();
        }
    }
}

class GoodTask implements Task 
{
    public function __construct(private string $name) 
    {
    }

    public function execute(): void {
        echo "Executing task: {$this->name}\n";
    }
}

// Client code
$task1 = new GoodTask("Task 1");
$task2 = new GoodTask("Task 2");

$compositeTask = new CompositeTask();
$compositeTask->addTask($task1);
$compositeTask->addTask($task2);

// Clients can treat individual tasks or the composite task uniformly
$compositeTask->execute();
```

The refactored code, based on the Composite Pattern, introduces an interface (`Task`) and composite class (`CompositeTask`), allowing for a more scalable and flexible structure. Now, tasks can be treated uniformly whether they are individual tasks or groups of tasks.
