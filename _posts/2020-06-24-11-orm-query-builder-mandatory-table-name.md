---
layout: post
title:  "# 11 - V ORM v query builder treba vždy k názvu stĺpca pridať aj názov tabuľky"
date:   2020-06-24 16:16:00 +0200
categories: [Tech > PHP > CakePHP]
---

V CakePHP ORM v query builder treba vždy k názvu stĺpca pridať aj názov (alebo alias) tabuľky. Aby neskôr zbytočne nevznikali kolizie v názvoch stĺpcoch.

# Prečo je to dôležité?

Ak chceme neskôr pridať nejakým spôsobom do query `contain` (*join* na inú tabuľku), tak treba ku stĺpcom manuálne dopĺňať názov tabuľky.

Ešte horšie je to v prípade, že sa `contain` pridá hromadne na veľa miest. Napríklad ak sa pridá do *finder-a*, ktorý je použitý na viac miestach. Alebo cez `beforeFind` &ndash; tak to napríklad robí `TranslateBehavior`. Pri bežných stĺpcoch (napr. `id`, `created` alebo `modified`), ktoré má veľa tabuliek, potom databáza nevie, ktorú tabuľku má použiť.