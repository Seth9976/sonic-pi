A.15 Vijf Live codering technieken

# Vijf Live-Coding Technieken

In de Sonic Pi zelfstudie van deze maand gaan we eens kijken hoe je Sonic Pi als een echt muziekinstrument kan gaan hanteren. We moeten daarvoor met een andere blik naar de code beginnen kijken. Live Coders bekijken hun code zoals een violist naar zijn strijkstok kijkt. Zoals een violist in feite verschillende strijktechnieken gebruikt om andere klanken te creëren (lange trage bewegingen of korte snelle aanslagen), zullen wij vijf van de basistechnieken onderzoeken die Sonic Pi mogelijk maakt. Aan het eind van het artikel, kan je je eigen live coding optreden beginnen oefenen.

## 1. Het onthouden van de Sneltoetsen

De eerste tip om met Sonic Pi live coding te gaan doen is beginnen gebruik maken van de sneltoetsen. Bijvoorbeeld, in plaats van kostbare tijd te verliezen door de muis te moeten gaan nemen om op de afspeeltoets te klikken kan je simpelweg tegelijk de `alt` en`r` toets typen, dat werkt niet alleen sneller, je vingers staan zo ook al klaar op het klavier voor de volgende bewerking. Je kan de sneltoetsen voor de belangrijkste knoppen bovenaan makkelijk uitzoeken door met de muis eroverheen te zweven. Zie sectie 10.2 voor de ingebouwde handleiding voor de volledige lijst met sneltoetsen.

Wanneer je een voorstelling doet is het ook prettig om je armbewegingen met wat flair te doen als je een sneltoets aanslaat. Zo is het bijvoorbeeld heel goed met het publiek fysiek te communiceren als je op het punt staat een verandering te maken, dus maak een mooie beweging als je `alt-r` typt, net zoals een gitarist zou doen wanneer hij een power chord aanslaat.

## 2. Handmatig je geluid van lagen voorzien

Nu je de code direct kunt activeren met het computerklavier, kan je deze vaardigheid voor onze tweede techniek gaan gebruiken, en dat is handmatig je geluid van lagen voorzien. In plaats van te componeren door meermaals een `play`, en een `sample` op te roepen, gescheiden door oproepen van `sleep`, hebben we één oproep naar `play` welk we manueel activeren met `alt-r`. Typ de volgende code in een nieuwe buffer:

```
synth :tb303, note: :e2 - 0, release: 12, cutoff: 90
```

Klik nu op de `Afspeel` knop wanneer de klank loopt, verander de code om vier tonen te zakken door het volgende te doen:


```
synth :tb303, note: :e2 - 4, release: 12, cutoff: 90
```

Klik nu terug op `Afspelen`, om beide klanken, tegelijk te horen spelen. Dit komt omdat de `Afspelen`knop van Sonic Pi niet wacht tot een voorgaande code is afgelopen, maar in plaats daarvan de code tegelijkertijd start. Dit betekent dat je makkelijk veel lagen van je geluid van kleinere of grotere veranderingen kan voorzien tussen elke trigger. Verander bijvoorbeeld zowel de `note:` als de `cutoff:` opties en herstart dan.


Je kan deze techniek ook gebruiken met lange abstracte samples. Bijvoorbeeld:

```
sample :ambi_lunar_land, rate: 1
```

Probeer de sample op te starten en dan geleidelijk de `rate:` waarde te halveren tussen de afspeelcommando's door en gebruik getallen tussen `1` tot `0.5` tot `0.25` tot `0.125` en probeer zelfs negatieve waarden zoals `-0.5`. Voeg lagen toe en kijk waar je uitkomt. Voeg er tenslotte nog een snuifje FX aan toe.

Werken met eenvoudige regels code tijdens een uitvoering, betekent hier dat een publiek dat niet vertrouwd is met Sonic Pi, een goede kans maakt, te begrijpen wat jij aan het doen bent, en de code kunnen koppelen en lezen aan de klank die ze horen.


## 3.Live Loops Masteren

Als je eerder met ritmische muziek werkt, kan het dikwijls moeilijk zijn om alles manueel te activeren en hierin een goeie timing te houden. In plaats daarvan is het dikwijls beter om een `live_loop`te gebruiken. Het geeft herhaling aan je code en geeft je ook de mogelijkheid de code te bewerken voor de volgende cyclus van een loop. Deze lopen samen met andere `live_loop`s, wat betekent dat je deze kan lagen met elkaar en manueel triggers codeert. Bekijk sectie 9.2 van de ingebouwde handleiding om meer over live loops te weten te komen.

Denk eraan om, bij een uitvoering, gebruikt te maken van de `sync:` optie van `live_loop`om te kunnen herstellen van een accidentele doorloop die de live loop stopt door een fout. Als je de `sync:` optie laat verwijzen naar een andere geldige `live_loop`, kan je snel de fout herstellen en de code te herstarten zonder een tel te missen.

## 4. De Master Mixer gebruiken

Een van Sonic Pi's best bewaarde geheimen is dat deze een master mixer heeft waar het geluid doorgaat. Deze mixer heeft zowel een low pass als een high pass filter ingebouwd, zodat je makkelijk het globale geluid kan modificeren. De functies van de master mixer is toegankelijk via de fn `set_mixer_control!`. Probeer bijvoorbeeld het volgende, terwijl je de code loopt en hoort in een vrije buffer en druk op `Afspelen`:

` set_mixer_control! lpf: 50 `

Nadat je deze code uitvoert, worden alle bestaande en nieuwe geluiden van een low pass filter voorzien en daarom zal het geluid nu meer gedempt zijn. Merk op, dat dit betekent, dat de waarden van nieuwe mixer blijvend zijn, totdat ze weer veranderd worden. Je kan echter, als je wil, altijd opnieuw de mixer terug naar de standaard instelling zetten met `reset_mixer!`. Een aantal van de huidige ondersteunde opts zijn: `pre_amp:`, `lpf:` op hpf:` en `amp:`. Voor de volledige lijst, zie de ingebouwde handleiding voor `set_mixer_control!`.

Gebruik de `*_slide` opts van de mixer om tussen een of meerdere opt waarden te schuiven. Om bijvoorbeeld langzaam de waarde van de mixer's low pass filter van de huidige waarde naar 30 te brengen, gebruik het volgende:

```
set_mixer_control! lpf_slide: 16, lpf: 30
```

Je kan dan snel terug naar boven schuiven voor een hogere waarde met:

```
set_mixer_control! lpf_slide: 1, lpf: 130
```

Als u optreed is het nuttig om een lege buffer vrij te houden om zoals hier met de mixer te werken.

## 5. Oefening

De meest belangrijke techniek voor live coding is oefening. Het meest gezamenlijke kenmerk van professionele muzikanten is dat ze het bespelen van hun instrumenten - vaak vele uren per dag, oefenen. Oefening is net zo belangrijk voor een live-coder als voor een gitarist. Oefening laten uw vingers bepaalde patronen en veel voorkomende bewerkingen onthouden, zodat je deze kan typen, en je meer vloeiend met hen kan werken . Het oefenen geeft je ook de mogelijkheid nieuwe klanken en constructies te kunnen gaan verkennen .

Bij het uitvoeren, zal je zien dat hoe meer oefening je doet, hoe makkelijker het voor je zal zijn om te ontspannen bij het optreden. Oefening zal je ook een schat aan ervaring geven, om uit te putten. Dit kan je helpen te begrijpen welke soorten wijzigingen interessant zullen zijn en goed werken met de huidige "sound".

## Waardoor het allemaal samen valt

In plaats van je een laatste Voorbeeld te geven, waarin alle aangeleerde dingen worden gecombineerd, laten we deze maand afscheid nemen door je een uitdaging te laten aangaan. Kijk of je een week lang al deze ideeën kan oefenen. Bijvoorbeeld, de ene dag oefen je alle manuele triggers, de volgende dag wat `live_loop` basiswerk en de volgende dag speel je met de master mixer. Dan herhaal je dit. Maak je geen zorgen dat dit eest met horten en stoten gaat, blijf oefenen en voor je het weet ben je echt voor een publiek aan het live coden.
