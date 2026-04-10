# phpnomad/enum-polyfill

[![Latest Version](https://img.shields.io/packagist/v/phpnomad/enum-polyfill.svg)](https://packagist.org/packages/phpnomad/enum-polyfill)
[![Total Downloads](https://img.shields.io/packagist/dt/phpnomad/enum-polyfill.svg)](https://packagist.org/packages/phpnomad/enum-polyfill)
[![PHP Version](https://img.shields.io/packagist/php-v/phpnomad/enum-polyfill.svg)](https://packagist.org/packages/phpnomad/enum-polyfill)
[![License](https://img.shields.io/packagist/l/phpnomad/enum-polyfill.svg)](https://packagist.org/packages/phpnomad/enum-polyfill)

`phpnomad/enum-polyfill` provides a single `Enum` trait that gives any class with constants the method surface of a PHP 8.1 native enum: `cases()`, `from()`, `tryFrom()`, and `isValid()`. It exists so projects targeting PHP 7.4 or 8.0 can use the same enum patterns that ship natively in 8.1+, with a clear path to swap in real enums later.

## Installation

```bash
composer require phpnomad/enum-polyfill
```

## Quick Start

```php
use PHPNomad\Enum\Traits\Enum;

class Status
{
    use Enum;

    public const Active = 'active';
    public const Pending = 'pending';
    public const Inactive = 'inactive';
}

Status::cases();              // ['active', 'pending', 'inactive']
Status::isValid('active');    // true
Status::tryFrom('banned');    // null
Status::from('active');       // 'active'
```

## Overview

- `cases()` and its alias `getValues()` return every constant value on the class as an array.
- `isValid($value)` runs a strict comparison against the constant values.
- `from($value)` returns the value if valid or throws `UnexpectedValueException`.
- `tryFrom($value)` returns the value if valid or `null`.
- Reflection runs once per class. Values are cached on a singleton via `phpnomad/singleton`.

One thing to know before you migrate: the trait returns the raw constant value (a string, an int, whatever the constant is), not a typed enum instance. When you later move a class to a native PHP 8.1 enum, call sites that were holding `'active'` will need to hold `Status::Active` instead.

## Documentation

Full docs at [phpnomad.com](https://phpnomad.com).

## License

MIT. See [LICENSE.txt](LICENSE.txt).
