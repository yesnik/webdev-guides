# Error in mobile validation

## Code with error

```php
if (empty($claim->info->mobile) || (new MobileWithLocalValidator())->validateValue($claim->info->mobile)) {
    ClaimsService::markClaimRejected($claim, self::ERROR_MOBILE_INCORRECT);
    return;
}
```

## Code without error

Unit test helped to find it during development.

```php
// Look at `!`
if (empty($claim->info->mobile) || !(new MobileWithLocalValidator())->validateValue($claim->info->mobile)) {
    ClaimsService::markClaimRejected($claim, self::ERROR_MOBILE_INCORRECT);
    return;
}
```
