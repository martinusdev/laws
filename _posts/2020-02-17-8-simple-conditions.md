---
layout: post
title:  "# 08 - Podmienky musia byť napísané jednoducho"
date:   2020-02-17 14:15:50 +0100
categories: [Apps]
---

Pod podmienkami sa myslia všetky *boolean expressions*. Komplikované podmienky sa ťažko čítajú, ťažko chápu a sú náchylné na chyby.

## Presné pravidlá

### 1. V jednej podmienke je zakázané miešať AND a OR operátory

Nesprávne:
```php
if (((!is_null($deliveryType) && $deliveryType->id == DeliveryType::POST_SK_FOREIGN) || (!is_null($deliveryAddress) && !($deliveryAddress->isSlovakia() || $deliveryAddress->isCzechRepublic()))) && (!is_null($paymentType) && $paymentType->isCash())) {
    // ...
}
```

Správne:
```php
$isDeliveryForeign = !is_null($deliveryType) 
    && $deliveryType->id == DeliveryType::POST_SK_FOREIGN;

$isAddressForeign = !is_null($deliveryAddress) 
    && !$deliveryAddress->isSlovakia() 
    && !$deliveryAddress->isCzechRepublic();

$isOrderForeign = $isDeliveryForeign || $isAddressForeign;

$isPaymentCash = !is_null($paymentType) && $paymentType->isCash();

if ($isOrderForeign && $isPaymentCash) {
    // ...
}
```