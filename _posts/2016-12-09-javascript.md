---
layout: page
title: "JavaScript"
category: kehitys
date: 2016-12-09 15:53:16
order: 3
---

### Suorituskyky

Suorituskykyisen JavaScriptin kirjoittaminen on erittäin tärkeää. Huono ja raskas JavaScript kuormittaa käyttäjän selainta ja saattaa jopa kaataa ohjelman. Mobiiliyhteyksillä tämä korostuu erityisesti, joten JS:n toiminnallisuus ja hyvä suorituskyky täytyy pitää mielessä koko kehitysprosessin ajan.

Monissa Geniemin projekteissa, kuten kaikissa WordPress-projekteissa, käytetään [jQueryä](http://jquery.com/). JQueryssä on monia hyviä puolia, kuten sen tarjoama AJAX-käsittely ja JS:n kääntyminen  selaimille yhtenäisellä tavalla ymmärrettäväksi koodiksi.

JQueryn kanssa voi kuitenkin helposti tehdä varsin raskasta koodia. Pitää olla tarkkana, ettei esim. hae jokaista DOM-elementtiä classinsa mukaan monta kertaa uudestaan saman koodin aikana, vaan huolehtii elementtien ajonaikaisesta cachetuksesta ja oikeaoppisesta DOM:in käsittelystä.

### Käytä jQueryä harkiten

Monissa tapauksissa jQueryn käyttö elementtien manipulointiin voi olla turhaa. Alla esimerkki, jossa elementin piilottamisen voi hoitaa vain JavaScriptillä, mikä on tehokkaampaa.

```javascript
document.getElementById( 'element' ).style.display = 'none';
```

vs.

```javascript
jQuery( '#element' ).hide();
```

### Elementin asetus jQuery-objektiksi

Kun jqueryllä haetaan DOMista elementti, tekee se siitä taustalla jQuery-objektin

```javascript
jQuery( '#menu' );
```

Vaihtoehtoisesti jQuerylle voi antaa manuaalisesti [kokoelman elementtejä](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection) tai [elementin](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement) vastaavanlaisen jQuery-objektin luomiseksi. Tämä on suorituskyvyllisesti [nopeampaa.](http://jsperf.com/wrap-an-element-or-html-collection-in-jquery)

```javascript
// Elementti haetaan ID:llä
jQuery( document.getElementById( 'menu' ) );

// Elementti haetaan käyttäen "querySelector"-metodia
// Classeille hyvä, joskin classien avulla EI kannata hakea elementtejä, jos ei ole pakko.
document.querySelector('#menu');
document.querySelectorAll('.menu-item');
```

### DOMin cachetus

```javascript

app.$element = jQuery( '#element' );

// Muuta koodia...

app.$element.addClass('element--state');
app.$element.children('#child-element');
// DOM node on cachessa, joten sitä ei haeta uusiksi joka kerta

```

### JS-malli

Geniem käyttää javascript-tiedostojensa koodin jäsentämiseen alla olevaa mallia. Ideana mallissa on se, että yhden sivun JS on yhden olion sisällä metodeina, ja olio palautetaan myös window:n alle. Täten voidaan helposti cachettaa olion käyttämät DOM-elementit ja hallita olion metodeja järjestelmällisesti.

Malli on kommentoitu ja se sisältää muutaman hyödyllisen esimerkin. Mallin koodi on projektien boilerplatejen JS-puolella valmiina.

```javascript

// Luodaan olio ja annetaan sille parametreinä käyttöön window, document ja jQuery. jQuery on mapatty dollarimerkkiin oletuksena.
window.SivunOlio = ( function( window, document, $ ){

    // Viittaus olioon sisäisesti.
    var app = {};

    // Olion muuttujien cachetusfunktio. DOM-elementit haetaan olion käyttöön ajonaikaiseen cacheen.
    app.cache = function(){

        // Muuttuja prefixataan dollarimerkillä, jotta tiedetään, että se on jQuery-objekti.
        app.$element            = $('#element').data('ajaxurl');
        // Muuttujiin voi asettaa mitä vain dataa.
        app.elementData         = $('#element').data('ajaxurl');
        app.boolean             = false;
    };

    // Olion init-metodi. Asetetaan funktio muuttujaan, mikä on suorituskykyisempää, kuin irtonaiset funktion deklaraatiot.
    app.init = function(){
        // Kutsutaan cachetusmetodia
        app.cache();
        // Kutsutaan olion metodia
        app.method1();
        // Asetetaan event handler cachetetulle elementille. Esimerkissä mukana parametrien välitys handler-metodille.
        app.element.on( 'click', { passedParameter: "value" }, app.method2 );

    };

    // Metodin koodi
    app.method1 = function () {

        // Koodia...
    };

    // Eventhandler -metodin koodi. Esimerkkinä event datan lisäksi metodin saama parametri
    app.method2 = function( e, functionParam1 ){

        e.preventDefault();
        e.stopPropagation();

        // Annettu event data muuttujaan.
        var param1 = e.data.passedParameter;

        // Annettu parametri muuttujaan.
        var param2 = functionParam1;

        // Koodia...

    };

    // Ajetaan olion init-metodi document readyssä.
    $(document).ready(function() {

        app.init();

    });

    // Palautetaan olio window:lle. Olioon viitataan muualla käyttäen: "window.SivunOlio".
    return app;

})( window, document, jQuery );
```

### Koodin kommentointi

Koodin kommentointi on yksi tärkeimmistä käytännöistä ylläpidettävyyden ja jatkokehitettävyyden kannalta. Kommentoimalla koodiasi kattavasti ja yksiselitteisesti varmistat, että toisetkin ymmärtävät mitä olet kirjoittanut. Kirjoita kommentteja koodiin mieluummin liikaa kuin liian vähän.

### Koodaustyylit

Geniemin JavaScript-koodaustyylejä valvotaan Geniemin [JSCS](http://jscs.info/)-konfiguraatiolla. Konfiguraatiotiedostoa käytetään Sublime Linterissä, jolloin koodaustyylivirheet näkee heti koodia kirjoittaessa. Katso ohjeet [työkalut]({{ site.baseurl }})-sivulta. Alla muutama poiminta linterin pakottamista säännöistä.

- Muuttujien ja metodien nimeämisessä käytetään camelCasea.
- Olioiden ja niiden metodien sekä muuttujien nimeäminen tulee olla kuvaavaa ja tarpeeksi tarkkaa.
- Rivinvaihtoja käytetään siten, että koodi on selkeästi jäsenneltyä.
- Välilyöntejä käytetään runsaasti, esim. metodin parametrien ja sulkujen välillä sekä if-ehtolausekkeissa.

