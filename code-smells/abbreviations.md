# Abbreviations

The idea behind this is not to use abbreviations in your code as they make your code less readable and can lead to errors if you are not focused enough.

### Before

```php
class SomeClass
{
    public function __construct(
        private readonly ContractRepository $cr,
        private readonly ContractAddressRepository $car
    ) {
    }

    public function doSomething(): void
    {
        $c = $this->cr->fetchById('1');
        $ca = $this->car->fetchByContractId($c->getId());
    }
}
```

### After

```php
class SomeClass
{
    public function __construct(
        private readonly ContractRepository $contractRepository,
        private readonly ContractAddressRepository $contractAddressRepository
    ) {
    }

    public function doSomething(): void
    {
        $contract = $this->contractRepository->fetchById('1');
        $contractAddress = $this->contractAddressRepository->fetchByContractId(
            $contract->getId()
        );
    }
}
```
