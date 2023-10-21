# Observer Pattern

## What is it?

The **Observer Pattern** is a behavioral design pattern where an object, known as the subject, maintains a list of its dependents, called observers, that are notified of any state changes. This pattern is used to establish a one-to-many relationship between objects, ensuring that when one object changes state, all its dependents are automatically updated.

## Where is it used?

The Observer pattern is widely used in scenarios where a change in one object triggers a cascading effect across multiple objects or components. Common use cases include:

1. **User Interface (UI) Development:** Updating UI elements in response to changes in underlying data.
2. **Event Handling Systems:** Notifying multiple listeners about events or changes.
3. **Distributed Systems:** Broadcasting updates to multiple components in a distributed system.
4. **Model-View-Controller (MVC) Architectures:** Keeping views synchronized with changes in the model.

## Advantages

- **Loose Coupling:** The subject and observers are decoupled, allowing changes in one without affecting the other.
- **Extensibility:** Easily add or remove observers without modifying the subject.
- **Reusability:** Reuse existing subject and observer classes in different contexts.

## Example

Consider a scenario where you have a `NewsPublisher` that publishes news articles, and various `NewsSubscribers` that need to be notified when a new article is published.

```php
class NewsPublisher 
{
    private array $subscribers = [];

    public function addSubscriber(mixed $subscriber) {
        $this->subscribers[] = $subscriber;
    }

    public function publishNews($news) {
        // Publishing news logic...

        foreach ($this->subscribers as $subscriber) {
            $subscriber->update($news);
        }
    }
}

class NewsSubscriber 
{
    public function update($news): void {
        // Update logic...
        echo "Received news update: $news\n";
    }
}
```

In this example, the code violates the principles of encapsulation and is tightly coupled, making it difficult to extend or modify.

**Refactored Code Using Observer Pattern:**

```php
interface ObserverInterface
{
    public function update(string $news): void;
}

class NewsPublisher 
{
    private array $subscribers = [];

    public function addSubscriber(ObserverInterface $subscriber): void {
        $this->subscribers[] = $subscriber;
    }

    public function publishNews(string $news): void {
        // Publishing news logic...

        foreach ($this->subscribers as $subscriber) {
            $subscriber->update($news);
        }
    }
}

class NewsSubscriber implements ObserverInterface 
{
    public function update(string $news): void {
        // Update logic...
        echo "Received news update: $news\n";
    }
}
```

In this refactored code, the Observer pattern is applied. The `NewsSubscriber` now implements the `ObserverInterface` interface, promoting loose coupling and allowing for easy extension and maintenance. The `update` method includes a simple logging statement to illustrate the update logic.

## Symfony solution

In a Symfony solution, you can leverage the Dependency Injection component to manage services and their dependencies. 

Here's an example of the Observer pattern in a Symfony context using tagged services and interfaces passed through the constructor.

```yaml
# config/services.yaml
_instanceof:
  App\Observer\NewsObserverInterface:
    tags: ['app.news_observer']
    lazy: true

services:
  App\Service\NewsPublisher:
    arguments: !tagged 'qpp.news_observer'
```

**Observer Interface:**

```php
// src/Observer/NewsObserverInterface.php
namespace App\Observer;

#[AutoconfigureTag('app.news_observer')]
interface NewsObserverInterface 
{
    public function update(string $news): void;
}
```

**Concrete Observer Subscriber**

```php
// src/Observer/NewsSubscriber.php
namespace App\Observer;

class NewsSubscriber implements NewsObserverInterface 
{
    public function update(string $news): void {
        // Update logic...
        echo "Received news update: $news\n";
    }
}
```

**NewsPublisher Service:**

```php
// src/Service/NewsPublisher.php
namespace App\Service;

class NewsPublisher 
{
    public function __construct(private iterable $observers) 
    {
    }

    public function publishNews(string $news): void {
        // Publishing news logic...

        foreach ($this->observers as $observer) {
            $observer->update($news);
        }
    }
}
```

In this example:

- `NewsObserverInterface` defines the contract for observers.
- The `NewsPublisher` service takes an iterable of tagged services (observers) through its constructor.
- `NewsSubscriber` is a concrete implementation of the observer interface, and it's tagged as `app.news_observer` in the Symfony service configuration.

This Symfony-based implementation adheres to best practices by leveraging the Symfony Dependency Injection component and service tags, promoting modularity and maintainability.
