# Complex Expression

Imagine you have a **complex expression** that is difficult to understand at first glance.

### Before

```php
if (($platform->toUpperCase()->indexOf("MAC") > -1)
    && ($browser->toUpperCase()->indexOf("IE") > -1)
    && $this->wasInitialized() 
    && $this->resize > 0
) {
  // do something
}
```

Such an expression increases the [cognitive load](https://github.com/zakirullin/cognitive-load) on any developer to easily understand the code.

It is better to define well-named variables, which helps developers to understand the expression very quickly and avoid mistakes when making changes.

### After

```php
$isMacOs = $platform->toUpperCase()->indexOf("MAC") > -1;
$isIE = $browser->toUpperCase()->indexOf("IE")  > -1;
$wasResized = $this->resize > 0;

if ($isMacOs && $isIE && $this->wasInitialized() && $wasResized) {
  // do something
}
```
