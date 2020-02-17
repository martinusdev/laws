---
layout: post
title:  "# 09 - Všetky návratové hodnoty funkcie majú rovnaký typ"
date:   2020-02-17 14:15:50 +0100
categories: [Tech]
---
Funkcie a metódy nesmú vracať viacero rôznych dátových typov.
Všetky možné návratové hodnoty každej funkcie musia mať 1 rovnaký typ.

## PHP

V PHP ľahko vynútiteľné cez type hinting, takže podzákon pre PHP: Každá PHP funckia/metóda musí mať return type hint.

Vrátiť 1 typ alebo `null` (v PHP type hinting s `?type`) je povolené, ale zváž radšej použiť [Null object pattern](https://en.wikipedia.org/wiki/Null_object_pattern) - tvoje API aj použivatelia tvojho API sa ti poďakujú. Príklad je `\Martinus\Cart\Lib\Postage\UnknownPostage`.

