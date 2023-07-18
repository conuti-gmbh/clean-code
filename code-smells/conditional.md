# Replace Conditional with Polymorphism

You have a conditional that performs various actions depending on object type or properties.

### Before

```php
class Bird {
  // ...
  public function getSpeed() {
    switch ($this->type) {
      case EUROPEAN:
        return $this->getBaseSpeed();
      case AFRICAN:
        return $this->getBaseSpeed() - $this->getLoadFactor() * $this->numberOfCoconuts;
      case NORWEGIAN_BLUE:
        return ($this->isNailed) ? 0 : $this->getBaseSpeed($this->voltage);
    }
    throw new Exception("Should be unreachable");
  }
  // ...
}
```

You can refactor it very easily and generate cleaner code, which is [Open-Closed](https://www.freecodecamp.org/news/open-closed-principle-solid-architecture-concept-explained/):

### After

```php
interface Bird {
  public function getSpeed();
}

class European implements Bird {
  public function getSpeed() {
    return $this->getBaseSpeed();
  }
}

class African implements Bird {
  public function getSpeed() {
    return $this->getBaseSpeed() - $this->getLoadFactor() * $this->numberOfCoconuts;
  }
}

class NorwegianBlue implements Bird {
  public function getSpeed() {
    return ($this->isNailed) ? 0 : $this->getBaseSpeed($this->voltage);
  }
}

// Somewhere in client code
$speed = $bird->getSpeed();
```

This way you can [encapsulate](https://stackify.com/oop-concept-for-beginners-what-is-encapsulation/) the logic in different classes for each bird species and also create new bird species without changing any other class. Means your code is [Open-Closed](https://www.freecodecamp.org/news/open-closed-principle-solid-architecture-concept-explained/): open for extension, but closed for modification.
