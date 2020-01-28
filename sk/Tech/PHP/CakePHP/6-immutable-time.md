# 6 - Time, Date a FrozenDate sú zakázané, vždy používať iba FrozenTime

`Time`, `Date` a `FrozenDate` sú zakázané. Vždy treba používať iba FrozenTime. Pretože:
* `Time` a `Date` sú mutable,
* `FrozenDate` a `Date` nepodporujú timezone-y a zároveň majú niektoré metódy, ktoré na timezone závisia (napr. `isToday()`), je to Chronos bug.

Výnimka:
1. Historicky `FrozenTime` neexistoval, písali sme kód, ktorý využíval mutable `Time`. Ak to vyžaduje existujúci kód, je povolené použiť `Time`. Týka sa to hlavne ORM.

## Ako najlepšie pracovať s existujúcim mutable Time?

```php
$entity = $this->Table->get($id);
$created = new FrozenTime($entity->created);
``` 

## Prečo používať immutable FrozenTime namiesto Time?

Názov je trochu menej elegantný ako `Time`, ale je menej náchylný na chyby, ak si ho zvykneme používať.

Vzorová situácia: `$tomorrow = $now->addDay();`

`Time` verzia: 
  - keďže `Time` je mutable, dá sa mu meniť hodnota,
  - volanie `addDay()` zmení hodnotu toho objektu, na ktorom sa zavolá - čiže hodnotu objektu `$now` posunie o 1 deň,
  - zároveň ten istý objekt priradíme do premennej `$tomorrow`,
  - výsledkom je, že `$now` aj `$tomorrow` budú ukazovať na 1 inštanciu, v ktorej bude zajtrajší dátum.

`FrozenTime` verzia:
  - keďže `FrozenTime` je immutable, nedá sa mu meniť hodnota,
  - volanie `addDay()` teda musí vytvoriť nový objekt, do ktorého nastaví hodnotu posunutú o 1 deň,
  - nový objekt sa priradí do premennej `$tomorrow`,
  - výsledkom je, že `$now` a `$tomorrow` budú 2 rôzne inštancie s rôznymi hodnotami, v jednej dnešný, v druhej zajtrajší dátum,
  - stane sa teda to, čo by sme na pohľad očakávali.

S `FrozenTime` ako základnou verziou sa vyhneme týmto chybám a o nič neprídeme.

Immutable object: https://en.wikipedia.org/wiki/Immutable_object 

## Aj na prácu s dátumom bez času treba používať FrozenTime

Chronos `Date` a `FrozenDate` nemajú timezone (vždy sa pri vytvorení použije UTC - Chronos 1, alebo timezone servera - Chronos 2). To z istého uhla pohľadu dáva zmysel. 14. marca je proste 14. marca v USA aj v Japonsku rovnako. 

Problém je, že `Date` a `FrozenDate` majú celé API rovnaké ako `Time` a `FrozenTime`. Vrátane takých funkcií, ktoré dátum porovnávajú s aktuálnym UTC dátumom a časom - napr. `isToday()`. 

```php
// now:
// 2020-01-28 00:30:00 Europe/Bratislava
// 2020-01-27 23:30:00 UTC

$date = new FrozenDate('2020-01-28'); 
// => 2020-01-28 00:00:00 UTC

$date->isToday(); 
// => false

// Vysvetlenie: implementácia isToday():
// return $this->toDateString() === static::now($this->tz)->toDateString();

$first = $date->toDateString(); 
// => 2020-01-28

$date->tz; 
// => UTC

$now = static::now($this->tz); 
// => 2020-01-27 23:30:00 UTC

$second = $now->toDateString(); 
// => 2020-01-27

$first === $second;
// => false
```

Výsledkom je bug, ktorý sa deje iba 1 hodinu denne, keď nikto nepracuje, a nedá sa dobre nasimulovať ani debugovať.

Čiže ak dátum uložíme do `FrozenTime` s časom `00:00:00` a so správnou timezone, vyhneme sa tomuto bugu.

## Ako pracovať s dátumami, ktoré nemajú timezone?

Napríklad narodeniny alebo účtovníctvo?

🤷‍♂️

Zákon nateraz nešpecifikuje. Niektoré z možností:

 - môžu ostať ako stringy,
 - alebo si spravíme vlastný Date,
 - alebo použijeme `FrozenTime` s nejakou rozumnou timezone (napr. rozumná timezone pre moje narodeniny je taká, kde práve žijem - narodeninový email by som nemal dostať skôr alebo neskôr).