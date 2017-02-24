---
layout: page
title: "Versionhallinta"
category: tyokalut
date: 2016-12-09 17:03:45
order: 3
---

### Pääperiaatteet

* Kirjoita koodi aina johonkin omaan featurebranchiin, muuhun kuin masteriin
* Nimeä featurebranchit Jirataskin mukaan mikäli mahdollista.
* Tagaa ```master``` branch kun on todettu, että toimiva liveytys on tehty. Kirjoita lyhyt kommentti tagaukseen, jossa liveytyksen syy, päivämäärä sekä versionumero.
* Käytä tagauksen versionumerointiin SEMVER numerointia [http://semver.org/](http://semver.org/) [https://git-scm.com/book/en/v2/Git-Basics-Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
* Kaikki branchit, joissa on uniikkia koodia tulee pushata originiin. Ei säilötä mitään koodia pelkästään lokaalissa gitissä.
* Mikäli on tarve tehdä hotfix, tee oma hotfix branch masterista. Laita branchin nimeen ```HOTFIX-yyyy-mm-dd``` tai ```HOTFIX-Jira task``` (jos sellainen on)”. Kommentoi pushiin lyhyesti syy hotfiksille ja mitä on tehty.
* ```staging``` branch mergetään ```master``` branchiin kun kaikki ```staging``` branchiin mergetyt feature branchit halutaan liveyttää.
* Mikäli ```staging``` branchista ei voida liveyttää kaikkea, masteriin tai väliaikaiseen branchiin mergetään halutut feature branchit ja ko. branch testataan dev tai stage palvelimella.

### Featurebranchit

Jokaista selkeää featurekokonaisuutta varten luodaan omat branchit joissa ko. featurea kehitetään.

### Lokaalit testausbranchit

Jokainen kehittäjä voi tehdä omia testibrancheja joihin voi mergetä featurebrancheja testimielessä. Nämä branchit ovat vain testausta varten, eikä niitä pushata originiin ellei ole erityistä tarvetta mm. testauksen siirtäminen toiselle henkilölle.

### Stage

Tässä branchissa on versio joka on ajossa sellainen versio joka sisältää devaajien testaamia featureita joita asiakas haluaa pitää staging palvelimella.

### Hotfix

Tämä branchi kloonataan masterista ja siihen tehdään suoraan tarvittava fiksaus. Hotfix voidaan deployata staging palvelimella ja testata siellä. Kun testaus on suoritettu staging palvelin palautetaan käyttämään ```stage``` branchia. Hotfix branch mergetään ```master``` branchiin ja depoloyataan uusi ```master``` branch live palvelimelle.

### Master

```master``` brachissa on live palvelimella kulloinkin oleva versio.

### Kehitys

Feature brancheihin kehitetään yksittäisiä featureita omalla koneella ja ne pushataan originiin. Lokaaleja testausbrancheja käytetään devauksen tukena. Nämä branchit tulee poistaa käytön jälkeen.

### Testaus

Kun on tehnyt featureita joita haluaa laittaa staging-palvelimelle, hakee ```staging``` branchin omalle koneelle, johon mergettää halutut feature branchit. Tämä branch testataan devi tai stage palvelimella projektista riippuen.

Kun branchiin on tyytyväinen se mergetään ```staging``` branchiin. ```staging``` branchiin mergetään sellaisia feature brancheja, jotka devaaja on itse testannut ja asiakas haluaa staging palvelimelle

### Code review

Projektin git-käytännöt asetetaan niin, että merge request on pakko tehdä, jos featuren haluaa asiakkaan nähtäväksi asti. Tämä taso vaihtelee projektin palvelin-/palvelu-arkkitehtuurin perusteella stagen ja tuotannon välillä.

### Liveytys

Jos todettu ```staging``` branch toimivaksi ja halutaan liveyttää se kokonaisuudessaan, mergetään se ```master```branchiin ja deployataan.

Jos halutaan liveyttää vain osa ```staging``` branchissa olevista feature brancheista, mergetään ```master``` branchiin ko. feature branchit.

### Workflow esimerkki Git-käskyillä

Esimerkkiprojektiin ```v1.0.0``` halutaan uusi ominaisuus, kehittäjä on saanut jiratehtävän ```TIL-123```

Kehittäjä hakee esimerkkiprojektin työasemalleen ```git clone git@mygitdomain.com:project/repository.git```

Kehittäjä luo featurebranchin ```git checkout -b TIL-123```

Kehittäjä koodaa uuden ominaisuuden ja tallentaa työnsä gitlabiin

```git add -A```

```git commit -m 'halutut ominaisuudet xyz'```

```git push origin TIL-123```

Kehittäjä saa ominaisuuden valmiiksi ja yhdistää sen ```stage`` branchiin testaamista varten

```git checkout stage```

```git merge TIL-123```

```git push origin stage```

Ominaisuus deployataan staging-palvelimelle, testataan ja asiakas hyväksyy sen.

Kehittäjä yhdistää ominaisuuden ```master```-branchiin

```git checkout master```

```git merge TIL-123```

```git push origin master```

```master```-branch deployataan tuotantopalvelimelle onnistuneesti, se testataan ja hyväksytään. Kehittäjä tägää ```master```-branchiin toimivan version

```git tag -a v1.1.0 -m 'julkaisu ominaisuuksilla xyz 2016-04-20'```

```git push --follow-tags```

### Rollbackit

Huomataan, että esimerkkiprojektin liveytys ei olekaan hyvä, uusi ominaisuus bugaa pahasti.

Kehittäjä palauttaa ```master```-branchin versioon v1.0.0

```git reset --hard v1.0.0```


