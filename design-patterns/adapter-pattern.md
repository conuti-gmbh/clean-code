# Adapter Pattern

## What is it?

The **Adapter Pattern** is a structural design pattern that allows the interface of an existing class to be used as another interface. It enables incompatible interfaces to work together, providing a bridge between them.

## Where is it Used?

- **Legacy Code Integration:** When you need to incorporate existing classes with incompatible interfaces into new systems.

- **Library Usage:** When you want to use a third-party library with an interface that doesn't match your application's requirements.

- **Interface Versioning:** To adapt an old interface to a new one without modifying existing code.

## Advantages

- **Reusability:** Allows the reuse of existing classes without modifying their source code.

- **Interoperability:** Enables components with different interfaces to work together seamlessly.

- **Versioning Support:** Facilitates the integration of new interfaces without affecting existing code.

## Example

Consider an old `Logger` class with a non-compliant interface:

```php
class OldLogger 
{
    public function logMessage($message) {
        echo "Old Logger: $message\n";
    }
}

// Usage in client code
$oldLogger = new OldLogger();
$oldLogger->logMessage("Log this message");
```

Let's enhance the code by applying the Adapter Pattern and developing an adapter to align with the new `LoggerInterface`:

```php
interface LoggerInterface 
{
    public function log($message);
}

class LoggerAdapter implements LoggerInterface 
{
    public function __construct(private OldLogger $oldLogger) 
    {
    }

    public function log($message) {
        $this->oldLogger->logMessage($message);
    }
}

// Usage in client code
$oldLogger = new OldLogger();
$loggerAdapter = new LoggerAdapter($oldLogger);
$loggerAdapter->log("Log this message using the new Logger");
```

In this refactored code, the `LoggerAdapter` allows the old `OldLogger` to conform to the new `LoggerInterface`, demonstrating the adaptability and improved maintainability of the code.
