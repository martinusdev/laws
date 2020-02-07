# 10 - Každý argument funkcie akceptuje iba hodnoty jedného typu

Každý argument funkcie alebo metódy musí mať definovaný iba 1 typ.
Funkcie/metódy teda nesmú pre 1 argument akceptovať hodnoty rôznych dátových typov (2 a viac rôznych dátových typov).

Zákon umožňuje 1 výnimku &ndash; ak akceptovanie viacerých rôznych typov zjednodušuje navrhnuté API. A to iba za podmienky, že autor preukázateľne dokáže, že funckia bude fungovať správne pre všetky možné primitívne typy a objekty. Správne fungovanie môže byť aj exception pre nepodporované typy argumentov. Najlepší spôsob, ako autor preukázateľne dokáže, že funckia bude fungovať, sú testy.

# PHP

V PHP ľahko vynútiteľné cez type hinting, takže podzákon pre PHP: Každý argument PHP funckie/metódy musí mať type hint.

Akceptovať 1 typ alebo `null` (v PHP type hinting s `?type`) je povolené.

