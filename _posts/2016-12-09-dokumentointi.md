---
layout: page
title: "Dokumentointi"
category: tyokalut
date: 2016-12-09 16:30:54
order: 4
---

### WordPress-projektien dokumentointi

WordPress-projekteissa projektin juureen luodaan ```README.md``` tiedosto, jossa kerrotaan projektin oleellisimmat asiat kehittäjälle paikallisen kehitysympäristön pystytykseen ja projektin stagelle ja tuotantoon viemiseen.

### Asiakokonaisuuksien dokumentointi

Projektin juureen luodaan ```docs``` hakemisto, jonka alle dokumentoidaan omiin .md tiedostoihinsa projektin oleellisimmat tekniset kokonaisuudet ja asiat.

Esim. tehty henkilöitä varten uusi ```Custom Post Type``` "henkilot" johon liittyy filtteröitävä listausnäkymä (```sivupohja```) sekä yksittäisen henkilön ```single``` näkymä. Tätä kokonaisuutta varten luodaan ```docs/henkilot.md``` johon kirjataan:

* Henkilöt kokonaisuuden toiminnallisuus (esim. vaatimusmäärittelystä)
* Sisältötyypissä käytetyt ```custom fieldit``` ja niihin liittyvät mahdolliset erityishuomiot
* Filtteröinnin toteutustapa, esim. javascript-kirjasto
* Tärkeimpien tiedostojen sijainnit
* Erityiset muut lisähuomiot, esim. custom rewritet

Minimissään jokaisesta asiakokonaisuudesta olisi dokumentoituna vähintään vaatimusmäärittelyn mukainen lopputulos ja poikkeukset tai jos vaatimusmäärittelyä ei ole niin lyhyt kuvaus toiminnallisuudesta.

### Koodin dokumentointi

Dokumentoidaan omat metodit [PHPDoc](https://en.wikipedia.org/wiki/PHPDoc) standardin mukaan.

