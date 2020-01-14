# 2 - Nepoužívať implicitné typové konverzie

Implicitné typové konverzie sú niekedy zvláštne. A aj keď sa objektívne dajú vysvetliť, málokto v nich má prehľad a nikto si na všetky prípady nedáva pozor.

Jeden príklad za všetky:
```php
var_dump("1" == "01"); // 1 == 1 -> true
```

Síce (subjektívne) zriedkavo, ale niekedy vyústia v zbytočné chyby, ktoré by sme nemuseli mať. Preto si **používanie implicitných konverzii typov zakazujeme**.

Pod implicitnými typovými konverziami myslíme:
1. [PHP operátory](https://www.php.net/manual/en/language.operators.php) sa môžu používať len s takými hodnotami/premennými, aby nedošlo k implicitnej typovej konverzii, napr.:
    * logické operátory `!`, `&&` alebo `||` sa môžu použiť iba s typom boolean,
    * aritmetické operátory `+`, `-`, `*` alebo `/` sa môžu použiť iba s typmi int alebo float,
2. porovnávacie operátory `==`, `!=` a `<>` sú zakázané, treba namiesto nich použiť `===` a `!==`,
3. zakázané sú aj verzie funkcií, ktoré robia implicitné typové konverzie, 
    * napr. `in_array($a, $b)` alebo `in_array($a, $b, false)`, povolené je iba volanie `in_array($a, $b, true)`,
4. zakázané `empty()` - premenná by mala mať hodnoty iba v 1 type - tým pádom sa pre každý typ dá napísať "empty()" podmienka ručne/explicitne; a na overenie, či existuje kľúč v poli treba použiť `isset()`,
5. `isset()` sa môže použiť len na overenie, či v poli existuje kľúč (alebo field v objekte, alebo lokálna premenná, ...), ak kľúč/field/premenná existuje, na overenie jej hodnoty treba použiť `=== null`, `!== null`, alebo `is_null()`.

Zákon umožňuje 1 výnimku - a to ak implicitné typové konverzie zjednodušujú navrhnuté API knižnice/modulu/triedy alebo implementáciu. A to iba za podmienky, že autor preukázateľne dokáže, že funkcia bude fungovať správne pre všetky možné typy/konverzie. Správne fungovanie môže byť aj exception pre nepodporované typy/konverzie. Najlepší spôsob, ako autor preukázateľne dokáže, že funkcia bude fungovať správne, sú unit testy.

Referencie:
* [PHP Type Juggling](https://www.php.net/manual/en/language.types.type-juggling.php)
* [PHP Type Comparison Tables](https://www.php.net/manual/en/types.comparisons.php)
* [PHP Operators](https://www.php.net/manual/en/language.operators.php)
