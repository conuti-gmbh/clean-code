# Feature Envy

**Feature Envy** occurs when a method in one class seems more interested in the data of another class rather than its own. In other words, it suggests that one class is making excessive use of the methods or properties of another class, possibly indicating a better location for the method.

### Before

```php
class ShoppingCart 
{
    private array $items = [];

    public function addItem(Item $item) {
        $this->items[] = $item;
    }

    public function calculateTotalWithTax(TaxCalculator $taxCalculator): float {
        $subtotal = 0;

        foreach ($this->items as $item) {
            $subtotal += $item->getPrice();
        }

        $tax = $taxCalculator->calculateTax($subtotal);

        return $subtotal + $tax;
    }
}

class TaxCalculator 
{
    public function calculateTax(float $amount): float {
        return $amount * 0.1; // Assume a 10% tax rate for simplicity
    }
}

class Item 
{
    private float $price;

    public function getPrice(): float {
        return $this->price;
    }
}
```

In this example, the `ShoppingCart` class is accessing the `getPrice` method of the `Item` class to calculate the total with tax. This might indicate that the logic for calculating the total with tax could be better placed within the `Item` class or a separate service.

### After

```php
class ShoppingCart 
{
    private array $items = [];

    public function addItem(Item $item) {
        $this->items[] = $item;
    }

    public function calculateTotalWithTax(TaxCalculator $taxCalculator): float {
        $subtotal = 0;

        foreach ($this->items as $item) {
            $subtotal += $item->calculateTotalWithTax($taxCalculator);
        }

        return $subtotal;
    }
}

class TaxCalculator 
{
    public function calculateTax(float $amount): float {
        return $amount * 0.1; // Assume a 10% tax rate for simplicity
    }
}

class Item 
{
    private float $price;

    public function getPrice(): float {
        return $this->price;
    }

    public function calculateTotalWithTax(TaxCalculator $taxCalculator): float {
        $tax = $taxCalculator->calculateTax($this->getPrice());
        
        return $this->getPrice() + $tax;
    }
}
```

**Explanation:**

In the fixed code:

1. The `calculateTotalWithTax` method in the `ShoppingCart` class now delegates the calculation to the `Item` class by calling `calculateTotalWithTax` on each item.

2. The `calculateTotalWithTax` method is introduced in the `Item` class, encapsulating the logic related to calculating the total with tax for an individual item.

This way, the responsibility for calculating the total with tax is moved closer to the data it depends on, reducing Feature Envy and making the code more maintainable. Each class now has a method that primarily works with its own data, leading to better encapsulation and a clearer separation of concerns.

## Comparison with "Tell don't Ask"

"Feature Envy" and "[Tell, Don't Ask](tell-dont-ask.md)" are related concepts, and they both point to issues related to the design and organization of code. While they share some similarities, they address slightly different concerns.

**Feature Envy:**
- Feature Envy specifically refers to a situation where a method in one class seems more interested in the data of another class rather than its own. It suggests that a method is making excessive use of the methods or properties of another class, possibly indicating a better location for the method.

**Tell, Don't Ask:**
- "Tell, Don't Ask" is a design principle that encourages you to tell objects what to do rather than asking them about their state and then deciding what to do. It promotes encapsulation and a more object-oriented approach. Instead of querying an object for its state and making decisions based on that state, you should tell the object to perform an action.

While addressing Feature Envy may involve moving methods closer to the data they operate on (similar to "Tell, Don't Ask"), the primary emphasis of "Tell, Don't Ask" is on the overall design philosophy, encouraging you to encapsulate behavior within objects rather than extracting data and making decisions externally.
