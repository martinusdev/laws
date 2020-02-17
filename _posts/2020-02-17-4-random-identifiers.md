---
layout: post
title:  "# 04 - Náhodné identifikátory majú minimálne 22 znakov v 64 znakovej abecede"
date:   2020-02-17 14:15:50 +0100
categories: [Apps]
---
128 bitové náhodné číslo (inými slovami 128 bitov entropite) je dosť na to, aby sa nedali nikdy<sup>1</sup> vyskúsať všetky možnosti.

To vychádza na 22 znakov v 64 znakovej abecede. Lebo 64 znakov je 6 bitov (2^6 = 64). 6 bitov * 22 znakov = 132 náhodných bitov (na porovnanie, 6 bitov * 21 znakov je iba 126 bitov). 

Dôležitý predpoklad je, aby boli naozaj náhodné.

V PHP treba použiť funkciu `openssl_random_pseudo_bytes`.
V Martinus PHP na to máme class `\Martinus\Utils\Code\StringIdentifiersGenerator`.

Príkladom 64 znakovej abecedy je base 64: `a-z`, `A-Z`, `0-9`, `+`, `/`.
Pre potreby URL sa viac hodí nahradiť znaky `+` a `/` za niečo iné.  StringIdentifiersGenerator používa znaky `-` a `_`.
V 16 znakovej abecede (hexadecimálne čísla) musí byť dĺžka strignu 32 znakov (16 = 2^4, 32 * 4 = 128).


Referencie:
* https://security.stackexchange.com/questions/6141/amount-of-simple-operations-that-is-safely-out-of-reach-for-all-humanity/6149#6149
* https://en.wikipedia.org/wiki/Password_strength 

---

<sup>1</sup> 128 náhodných bitov je dosť preto, lebo iba samotná iterácia všetkých možností bude podľa odhadov v roku 2040 vyžadovať všetku energetickú produkciu Zeme a trvať 10 rokov.

<sup>2</sup> Tip: je to dobré pravidlo aj pre náhodne generované heslá.

