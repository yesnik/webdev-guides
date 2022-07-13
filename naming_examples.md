# Naming examples

Right names improve our code's readibility.

## Method names

```php
// Bad
(new Date)->add(2);

// Good
(new Date)->addMonths(4);
```

```php
// Bad
function calculate()
{
    return ($addTime * $rate) + $number * $level;
}

// Good
function getMonthSalary()
{
    return ($overtime * $overtimeRate) + $days * $gradeRate;
}
```
