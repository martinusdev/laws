# 3 - Na ukladanie časov a dátumov do DB používame typ TIMESTAMP alebo typ DATETIME s hodnotami v UTC

Existujú len 2 možnosti ako správne ukladať do databázy dátumy a časy:
1. `TIMESTAMP` - lebo MySQL/MariaDB interne ukladajú hodnotu nezávislú na timezone (TODO ako?),
2. `DATETIME` s hodnotami v UTC.

Je to dôležité pre ďalšie operácie s dátami (napr. počítanie časového rozdielu medzi udalosťami, alebo zoraďovanie podľa toho, kedy udalosť nastala).

## Prečo je DATETIME v UTC nezávislý na timezone?
> We use UTC because it has an offset of 00:00: in other words, it has no timezone offset. Other timezones are offset from UTC, not the other way around.

Akákoľvek iná timezone môže mať offset a ten sa môže meniť (jednorazovo sa región presunie do inej tz, alebo pravidelne - letný čas). Pri ukladaní hodnôt v inej tz sa teda môžu nejaké časy preskočiť, alebo opakovať. Operácie s takými dátami sa budú robiť ťažko.

Pri preskočení času o 1 hodinu dopredu bude napr. nesprávny rozdiel medzi časmi. Reálne uplynulo 5 minút, rozdiel ale bude 1 hodina a 5 minút.
Pri posunutí času o 1 hodinu dozadu sa nebudú dať časy správne chronologicky zoradiť.

UTC timezone má jediná offset 0 a navždy bude mať.

## Povolené výnimky

1. Dátumy a časy nezávislé na timezone, napríklad:
    * dátum narodenia, narodeniny (12.4.1985) - bez ohľadu na timezone, je to vždy konkrétny dátum/rok,
    * sviatky, inak významné dátumy (4.7., 24.12., ...),
    * (fiktívny príklad) začiatok platnosti nového zákona v EU (v každej krajine začne platiť 1.10.2025 0:00 podľa lokálnej timezone krajiny) a pod.

2. Každý záznam/riadok ma vlastnú timezone:
    * tabuľka musí obsahovať stringové fully qualified timezone name v ďalšom stĺpci.

### Prečo stringové fully qualified timezone name?

> Instead of a numeric offset, use a string. Specifically, use the full qualified name in the Olson database, so something like Australia/Adelaide or America/Los_Angeles. These are standardized descriptors of the timezones used in the world, and you can use these in pretty much every programming language ever used in the last few decades.

Zlý spôsob: 
> One way to do this is to treat this column as an integer; say, store -240 in a row to represent a shift of 4 hours (4*60 minutes). That’s cool, and it does get you closer, but again, we’re talking about a use case where it actually does matter to be as accurate as possible (and being off an hour might lead to nonsensical data). Just having a numerical offset doesn’t give you fidelity of location. Was it during Daylight Saving Time? What about comparing that to today’s time; do we account for DST or not? Also, do we need to account for changes in timezones that might have happened since that point in time?

https://zachholman.com/talk/utc-is-enough-for-everyone-right#properly-storing-timezone-aware-times 