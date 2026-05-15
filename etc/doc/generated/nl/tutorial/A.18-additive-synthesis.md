A.18 Sound Design - Additieve Synthese

# Additieve Synthese

Dit is de eerste van een reeks korte artikeltjes over het gebruik van Sonic Pi bij klankontwerp (Sounddesign). We nemen een rondleiding door een aantal technieken die voor je beschikbaar zijn om je eigen unieke klank te ontwikkelen. De eerste techniek die we zullen bekijken heet * additieve synthese *. Dit kan ingewikkeld klinken - maar als we elk woord even apart bekijken is de betekenis vrij snel duidelijk. Ten eerste, additief betekent letterlijk de combinatie van een aantal elementen en het tweede woord synthese, betekent geluid creëren . Additieve synthese betekent daarom niets minder dan * bestaande geluiden combineren om nieuwe te maken *. Deze synthese techniek dateert al van lang geleden - bijvoorbeeld pijporgels in de middeleeuwen hadden veel lichtjes verschillende cillinderpijpen die je met tussenpozen kan in- of uitschakelen. De stop uit een bepaalde pijp trekken 'voegt deze aan de mix toe' en maakt het geluid rijker en complexer. Nu, laten we eens kijken hoe we dat kunnen doen met Sonic Pi.


## Eenvoudige combinaties

Laten we beginnen met het meest elementaire geluid dat er is - de eenvoudige zuiver gestemde sinus golf:

```
synth :sine, note: :d3
```

Laten we eens kijken hoe dit klinkt gecombineerd met een blokgolf:

```
synth :sine, note: :d3
synth :square, note: :d3
```

Merk op hoe deze twee geluiden combineren tot een nieuwe, rijker geluid. Natuurlijk, hoeven we hier nog niet te stoppen, we kunnen zoveel geluiden we willen toevoegen. We moeten echter wel voorzichtig zijn hoe veel geluiden we bij elkaar gaan optellen. Net zoals wanneer we verf maken om nieuwe kleuren te mengen, het toevoegen van dat beetje teveel kleur zal resulteren in bijvoorbeeld een rommelig bruin, evenzo - zal het toevoegen van te veel geluiden resulteren in een modderig geluid.


## Mengen

Laten we iets toevoegen om het geluid een beetje helderder te maken. We zouden een driehoeksgolf kunnen gebruiken van een octaaf hoger (voor dat hoge heldere geluid) voorlopig op amp '0.4' afgesteld om iets extra's toe te voegen aan het geluid, zo dat dit geluid het niet overheerst:

```
synth :sine, note: :d3
synth :square, note: :d3
synth :tri, note: :d4, amp: 0.4
```

Nu, probeer je eigen geluid te creëren door een combinatie van 2 of meer synths op verschillende octaven en amplitudes. Merk ook op dat je met elke synth kunt spelen en elk brongeluid afzonderlijk kan worden gewijzigd voordat het wordt gemengd in nog meer combinaties van geluiden.


## Detuning (Ontstemmen)

Tot nu toe hebben we bij het combineren van onze verschillende synthesizers dezelfde toonhoogte gebruikt of zijn we naar een andere octave overgestapt. Hoe zou het misschien klinken als wij ons niet aan octaven vasthouden, maar in plaats daarvan voor een iets hogere of lagere noot zouden kiezen? Laten we dit proberen:

```
detune = 0.7
synth :square, note: :e3
synth :square, note: :e3 + detune
```

Als we onze blokgolf met een 0.7 noot ontstemmen, klinkt dit misschien vals of niet in harmonie. Hoe dichter bij de 0 plaats je het geluid minder uit toon gezien de toon van de golven meer en meer met elkaar gaan samenvallen. Probeer dit zelf maar eens uit! Verander de waarde van de `detune:` opt van `0.7` naar `0.5`en luister hoe het geluid hierbij veranderen kan. Merk op dat lage ontstemde noten zoals bij `0.1`een heel fijn 'vet' geluid produceren en waarbij de lichtjes verschillende toonhoogten op een interessante wijze met elkaar een wisselwerking aangaan, dikwijls op zeer verrassende wijze .

Sommige van de ingebouwde synths beschikken reeds over een detune optie die exact dit toepast in één synth. Probeer te spelen met de ' detune: ' opt van ': dsaw ',': dpulse' en ': dtri'.


## Amplitude vormgeven

We kunnen ons geluid ook nog op een andere manier bijschaven door voor elke synth een andere envelope te gebruiken. Deze laten jou toe om sommige elementen van het geluid percussief te laten klinken terwijl we andere elementen langer kunnen laten naklinken.

```
detune = 0.1
synth :square, note: :e1, release: 2
synth :square, note: :e1 + detune, amp: 2, release: 2
synth :gnoise, release: 2, amp: 1, cutoff: 60
synth :gnoise, release: 0.5, amp: 1, cutoff: 100
synth :noise, release: 0.2, amp: 1, cutoff: 90
```

In het bovenstaande voorbeeld heb ik lawaaierige percussieve elementen gemixt met een meer aanhoudend achtergrond gerommel. Dit werd bereikt door twee noise synth's met een middelmatige cutoff waarde (`90`en `100`) door gebruik te maken van korte release-tijd waarden, samen met een ruis die een langere release-tijd heeft, met een lagere cutoff waarde (om het geluid minder scherp en meer rommelend te maken.)

## Waardoor het allemaal samen valt

Laten we deze technieken combineren om te zien of we gebruik kunnen maken van additieve synthese om een elementair bel geluid te re-creëren . Ik heb dit voorbeeld opgedeeld in vier secties. Als eerste hebben we het 'aanslag'-gedeelte, die het begin uitmaakt van onze bel klank - en dus gebruik maakt van een korte envelop (bijv. een `release:` van rondom `0.1`). Vervolgens hebben we het lange ring gedeelte waarin ik het pure geluid van de sinusgolf gebruik . Merk op dat ik vaak het verhogen van een noot toepas, met ongeveer `12` en `24`, dat het aantal noten met één a twee octaven verhoogd. Ik heb er ook een paar lage sinus golven aan toegevoegd die het geluid wat bas en diepte geeft. Tot slot, gebruik ik `define` om mijn code in een functie te wikkelen, die ik dan kan gebruiken om een melodie af te spelen. Probeer je eigen melodie uit en speel ook met de inhoud van de `:bell` functie totdat je je eigen leuk geluid hebt om mee te spelen!

```
Speel een melodie met onze nieuwe klok!
```
