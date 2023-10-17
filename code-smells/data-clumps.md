# Data Clumps

Data Clumps refer to a situation where groups of variables are frequently found together in your code, possibly being passed around as parameters or appearing together in various parts of your codebase. This smell indicates that these variables may be better off encapsulated in a separate class or structure, providing better organization and improved maintainability.

### Before

```php
// Bad Code
function calculateArea(float $length, float $width, float $height): array {
    $volume = $length * $width * $height;
    $surfaceArea = 2 * ($length * $width + $length * $height + $width * $height);
    
    return ['volume' => $volume, 'surfaceArea' => $surfaceArea];
}

// In various parts of the code...
$length = 5.0;
$width = 3.0;
$height = 2.0;

// ...

$result = calculateArea($length, $width, $height);
```

In this example, `$length`, `$width`, and `$height` are grouped together and passed as parameters to the `calculateArea` function, representing a Data Clump.

### After

```php
class Dimensions 
{
    public float $length;
    public float $width;
    public float $height;
}

/**
 * Calculate the volume and surface area based on dimensions.
 *
 * @param Dimensions $dimensions The dimensions of the object.
 * @return array{volume: float, surfaceArea: float} An associative array containing 'volume' and 'surfaceArea'.
 */
function calculateArea(Dimensions $dimensions): array {
    $volume = $dimensions->length * $dimensions->width * $dimensions->height;
    $surfaceArea = 2 * ($dimensions->length * $dimensions->width + $dimensions->length * $dimensions->height + $dimensions->width * $dimensions->height);

    return ['volume' => $volume, 'surfaceArea' => $surfaceArea];
}

// In various parts of the code...
$dimensions = new Dimensions();
$dimensions->length = 5.0;
$dimensions->width = 3.0;
$dimensions->height = 2.0;

// ...

$result = calculateArea($dimensions);
```

Here, the related variables `$length`, `$width`, and `$height` are encapsulated in a `Dimensions` class. The `calculateArea` function now accepts an instance of `Dimensions` as a parameter, providing better organization and reducing the likelihood of passing unrelated variables. This improves the readability and maintainability of the code.

## Difference from the Long Parameter List

There is a similar code smell [Long Parameter List](long-parameter-list.md) which is slightly different.

This code smell refers to situations where a function or method has a large number of parameters. Long parameter lists can make a function hard to read and maintain, and they may indicate that the function is doing too much or that there's an opportunity to encapsulate **related parameters** into a single object.
