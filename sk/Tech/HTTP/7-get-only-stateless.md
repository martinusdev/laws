# Stav meniace HTTP služby sa nesmú dať spustiť cez GET metódu

Zmena stavu je vykonanie nejakej trvalej zmeny/operácie. Štandardne tam patrí pridanie, editácia alebo zmazanie nejakého "resource".

HTTP služby spravené cez GET metódu by nemali robiť žiadne zo stav meniacich operácií, kvôli:
  1. náchylnosti na druhy chýb/probémov, ktoré nemáme pod kontrolou my,
  2. bezpečnosti používateľov.

## 1. Náchylnosť na chyby/problémy, ktoré nemáme pod kontrolou
HTTP služby spravené cez GET sú bežne interpretované ako read-only. Preto sa dajú link-ovať, bookmark-ovať, cache-ovať alebo refresh-ovať.
Predpoklad je, že GET služby/linky sa môžu bezstarostne spúštať dookola a nič sa nestane. Cache si napríklad môže načitať novú verziu stránky, prehliadač môže zavolať bookmarknutý link aby zobrazil používateľovi náhľad stránky. Používateľ môže dookola stáčať F5/refresh, atď. 

Ak teda GET služba bude meniť stav, môžu vznikať bugy, ktoré sa budú javiť vzácne a nevysvetliteľné. Napríklad používateľovi vždy záhladne zmiznú rovnaké 2 knižky z wishlistu, alebo produkty, ktoré používateľ nehodnotil, budú mať 5 hviezdičiek, atď. A jediná možná oprava je tak či tak iba prepísať operáciu na inú HTTP metódu než GET.

Aby sme teda zabránili neočakávanému správaniu, operácie, ktoré nejakým spôsobom menia stav, nesmú byť spustiteľné cez GET. Tieto operácie sa vďaka tomu nebudú dať link-ovať, bookmark-ovať, cache-ovať ani refresh-ovať. Používateľ musí explicitne stlačiť button na to, aby sa vykonali. 

## 2. Bezpečnosť používateľov
POST, PUT, PATCH, DELETE requesty vedia mať ďalšie úrovne ochrany - napr. CSRF prevention, form tampering prevention. GET requesty ich nevedia mať. Ak teda GET requesty menia stav, vie ich zneužiť útočník (napr. vložiť sebe do stránky ako obrázky linky, ktoré vymažú wishlisty na Martinuse, hodnotia nejaký produkt, ...). Jediné možné riešenie je prepísať operáciu na inú HTTP metódu než GET (a pridať CSRF ochranu).

## Povolené výnimky
1. Ak GET služba využíva nejakú formu autentifikácie/overenia, ktorá zabráni opätovnému vykonaniu danej služby - ideálne nejaký jednorazový token, ktorý nepoužitý expiruje - napríklad schválenie dovolenky z email-u alebo rozdelenie objednávky z email-u. Proste link sa bude dať použiť iba raz a nepoužitý link expiruje.
2. Ak je to žiadaná feature aplikácie, ktorá sa nedá implementovať inak - napríklad link na vloženie produktu do košíka alebo link na odhlásenie.