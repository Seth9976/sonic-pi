A.9 Randomiseren

# Surfen op Random Streams

In deel 4 van deze zelfstudie serie keken we even naar randomiseren terwijl we een aantal broeierige synth-riffs codeerden. Daar randomiseren zo'n belangrijk onderdeel van mijn live-coding DJ-sets is, dacht ik dat het zinvol was om de grondbeginselen ervan in detail te gaan bekijken. Laat ons beginnen surfen op Random Streams!

## Er is geen random

Het eerste wat je moet weten, en wat jou misschien ook verrast, wanneer je met de randomisering functies van Sonic Pi speelt, is dat deze niet écht random zijn. Wat betekent dit? Wel laat ons een paar tests doen. Verbeeld je eerst een getal tussen 0 en1. Onthou deze en verklap deze niet aan mij. Nu laat me raden... was het `0.321567`? Nee? Goh ik ben er blijkbaar toch niet zo goed in. Geef me nog een kans maar vraag deze keer Sonic Pi een nummer te kiezen. Start Sonic Pi v2.7+ en vraag deze een random nummer te genereren, maar ook nu verklap je deze niet aan mij:

```
print rand
```

En dan nu de onthulling... was het `0.75006103515625`? Yes! Ha maar ik merk dat je hier een beetje sceptisch tegenover staat. Misschien was het wel een gelukstreffer. We proberen nog een keer. Druk opnieuw op Afspelen en we zien wel wat we krijgen... Wat? Opnieuw `0.75006103515625`? Dit kan duidelijk geen random zijn! Je hebt gelijk, het is ook geen random.

Wat gebeurt hier? Determinisme is de dure computerwetenschappelijke term die hier kan opgaan.Het betekent gewoon dat het toeval niet bestaat en alles voorbestemd is om te worden. Uw versie van Sonic Pi is voorbestemd om altijd `0.75006103515625` terug te geven. Dit klink misschien helemaal niet nuttig, maar ik verzeker je dat dit één van de meest krachtige delen van Sonic Pi is. Als je hier aan vasthoud zal je leren vertrouwen te schenken aan de deterministische manier van randomisering in Sonic Pi als fundamentele bouwsteen voor jouw composities en live gecodeerde DJ sets.

## Een Random Melodie

Wanneer Sonic Pi opstart, laad het eigenlijk een opeenvolging van 441.000 pre-geregenereerde waarden in het geheugen op. Wanneer je een random functie zoals `rand` of `rrand` oproept, wordt deze stroom van random waarden gebruikt om jouw resultaat te regenereren. Elke oproep naar een random functie verbruikt een waarde van deze stroom. Daarom zal de 10-de oproep naar zo'n random functie, de 10-de waarde van de stroom gebruiken. Iedere keer je op de Afspelen toets drukt zal de stroom voor die doorloop gereset worden. Daarom ook kon ik het resultaat van `rand` voorspellen en was de 'random' melodie ook elke keer dezelfde. Ieders versie van Sonic Pi gebruikt dezelfde random stream, wat ook belangrijk is als we elkaars code onderling gaan delen.

Laten we deze kennis te gebruiken om een herhalende willekeurige melodie te genereren:

```
8.times do
 play rrand_i(50, 95)
 sleep 0.125
end
```

Typ deze in een lege buffer en klik Afspelen? Je hoort een melodie die bestaat uit 'random' noten tussen 50 en 95. Wanneer deze uitgespeeld is, klik je opnieuw op Afspelen opnieuw om exact dezelfde melodie te hoeren spelen.

## Handige randomisering Functies

Sonic Pi wordt geleverd met een aantal handige functies voor het werken met de random stream. Hier is een lijst met enkele van de meest nuttige:

* `rand` - geeft gewoon de volgende waarde van de stream weer
* `rrand` - geeft een random waarde binnen een bereik weer
* `rrand_i` - Geeft een random geheel getal binnen een bereik weer
* `one_in` - Geeft true of false weer met de gegeven probabiliteit
* `dice` - Immiteerd het rollen van een dobbelsteen en geeft waarden tussen 1 en 6 weer
* `choose` - Kiest een willekeurige waarde uit een lijst

Bekijk hun documentatie in het Help-systeem voor meer informatie en voorbeelden.

## Resetten van de Stream

Terwijl de mogelijkheid om het herhalen van een fragment met gekozen noten essentieel is om jouw riff te kunnen spelen op de dansvloer, is het misschien niet de riff die je wou. Zou het niet geweldig zijn moesten we een aantal verschillende riffs kunnen uitproberen om dan de beste ervan te kunnen uitkiezen? Hier gaat de toverdoos open.

We kunnen de stream met de fn 'use_random_seed' handmatig instellen. In Computerwetenschap, is random seed een startpunt waaruit een nieuwe stream van random waarden kunnen beginnen kiemen en bloeien. We proberen dit:

```
use_random_seed 0
3.times do
  play rrand_i(50, 95)
  sleep 0.125
end
```

Geweldig, we krijgen de eerste drie noten van onze random melodie hierboven: '84', '83' en ' 71'. We kunnen de seed naar ierts anders veranderen. Laat ons zeggen deze:

```
use_random_seed 1
3.times do
  play rrand_i(50, 95)
  sleep 0.125
end
```

Interessant, we krijgen '83', '71' en ' 61'. Je merkt hierbij misschien ook op dat de eerste twee cijfers hier, dezelfde zijn als de laatste twee in het vorige voorbeeld - dit is geen toeval.

Vergeer niet dat de random stream gewoon een gigantisch lijst is van vooraf op een rol gezette waarden is. Random seed gebruiken is gewoon een verspringen naar een plaats in de lijst. Een andere manier om ons dit voor te stellen is te denken aan een gigantische deck van door elkaar geschudde speelkaarten. Gebruik maken van random seed is als het openklappen van deze deck op een nauwkeurig punt. Het fabuleuze gedeelte hier is dat precies de mogelijkheid om rond te springen in de random stream, dit ons een krachtig instrument geeft wanneer we muziek maken.

Laten we onze random melodie van 8 noten nog eens opzoeken met de kracht van die nieuwe stream resetting, maar laten we er ook een live loop in gooien zodat we ermee kunnen experimenteren terwijl de code loopt:

```
live_loop :random_riff do    
  use_random_seed 0
  8.times do
    play rrand_i(50, 95), release: 0.1
    sleep 0.125
  end
end
```
  
Nu, terwijl deze nog speelt, verander je de seed waarde van `0`naar iets anders. Probeer `100`, en wat dacht je van `999`. Probeer ook je eigen waarden, experimenteer en speel er wat mee - en kijk welke seed de riff regenereerde die jij het leukst vindt.

## Waardoor het allemaal samen valt

De tutorial van deze maand was een behoorlijk technische duik in hoe de functionaliteit van de randomisatie in Sonic Pi werkt.Hopelijk gaf het jou wat inzicht hoe dit in z'n werk gaat en hoe je deze op een vertrouwde manier kan gaan gebruiken om repeteerbare patronen in jouw muziekstukken te creëren. Het is belangrijk je erop te wijzen dat je repeteerbare randomisatie *overal* je maar wil kan gebruiken. Je kan bijvoorbeeld de amplitude van noten randomiseren, de maat van het ritme, de hoeveelheid galm (reverb), de synth die je gebruikt, de mix van de FX, enz. enz. In de toekomst gaan we uitgebreid kijken naar sommige van die toepassingen, maar momenteel geef jou nog even dit voorbeeld.

Typ het volgende in een lege buffer, toets Afspelen, en begin dan links en rechts de seeds te veranderen, duw opnieuw op Afspelen (terwijl het nog speelt) en onderzoek welke verschillende geluiden, ritmes en melodieën die je kan maken. Wanneer je iets lekkers gevonden hebt, probeer dan het seed getal te onthouden, zodat je die later opnieuw kan gebruiken. Als je tenslotte een aantal leuke seeds gevonden hebt, nu kan je voor je vrienden een live coded optreden geven door gewoon te switchen tussen jouw favoriete seeds om een compleet muziekstuk te creëren.

```
live_loop :random_riff do
  use_random_seed 10300
  use_synth :prophet
  s = [0.125, 0.25, 0.5].choose
  8.times do
    r = [0.125, 0.25, 1, 2].choose
    n = (scale :e3, :minor).choose
    co = rrand(30, 100)
    play n, release: r, cutoff: co
    sleep s
  end
end
live_loop :drums do
  use_random_seed 2001
  16.times do
    r = rrand(0.5, 10)
    sample :drum_bass_hard, rate: r, amp: rand
    sleep 0.125
  end
end
```
