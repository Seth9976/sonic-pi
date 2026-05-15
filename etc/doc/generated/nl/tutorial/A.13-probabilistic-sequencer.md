A.13 Codeer een probabilistische Sequencer

# Codeer een probabilistische Sequencer

In een vorige aflevering van deze Sonic Pi serie, verkenden we de kracht van randomiseren om verscheidenheid,variatie en verassing te kunnen introduceren in onze live gecodeerde nummers en optredens. We plukten bijvoorbeeld willekeurig noten van een bepaalde toonladder, om nooit-eindigende melodieën te creëren. Vandaag gaan we een nieuwe techniek leren, die gebruik maakt van het randomiseren van ritme - probabilistische beats!

## Probabiliteit

Voor we nieuwe beats en synth ritmes beginnen maken, moeten we even kijken naar de basisprincipes van probabiliteit. Het lijkt misschien ingewikkeld, maar het is echt net zo eenvoudig als een dobbelstenen werpen - echt waar! Als je een 6 zijdige dobbelsteen gooit, wat gebeurt er dan? Wel misschien gooi je een 1, 2, 3, 4, 5 of 6 met precies dezelfde kansen.
Hierdoor heb je een kans van 1 in 6 van het gooien van een 1. We kunnen ook het gooien van dobbelstenen in Sonic Pi emuleren met de fn 'dice' . Laten we eens 8 keer gooien:

```
8.times do
  puts dice
  sleep 1
end
```

Kijk hoe in het logboek waarden tussen 1 en 6 wordt afgedrukt, alsof we een echte dobbelsteen geworpen zouden hebben.

## Random Beats

Beeld je even in dat je een trommel hebt en dat je elke keer je deze zou slaan, je een teerling zou werpen. Als je één gooit zou je hem slaan en elke worp met een ander getal niet. Je hebt nu een probalistische drummachine met een probabiliteit van 1/6! Laten we eens horen hoe dat klinkt:

```
live_loop :random_beat do
  sample :drum_snare_hard if dice == 1
  sleep 0.125
end
```


Laat ons even snel elke regel overlopen om er zeker van te zijn dat alles duidelijk is. Eerst creëren we een nieuwe `live_loop` die `:random_beat`heet , en de twee regels tussen `do` en `end`, herhaalt. De eerste regel roept een `sample` aan die een vooraf opgenomen geluidsbestand afspeelt ( `:drum_snare_hard` in dit geval ).Deze lijn heeft echter een special voorwaardelijk einde met de `if` functie. Deze betekent dat de regel enkel wordt uitgevoerd als de declaratie, rechts van de `if` (als), `true`(waar) is. Deze geeft in dit geval `dice == 1` aan. Die roept onze `dice`functie aan dewelke, zoals we reeds zagen, een waarde tussen 1 en 6 zal teruggeven.

## Probabiliteit veranderen

Wie al een rollenspel heeft gespeeld zal al wel vertrouwd zijn met vreemd gevormde dobbelstenen die verschillende reeksen getallen kunnen hebben. Er is bijvoorbeeld de tetraëder-vormige dobbelsteen die 4 zijden heeft en zelfs een 20 zijdige dobbelsteen in de vorm van een icosaëder. Het aantal zijden op de dobbelsteen verandert de kans of waarschijnlijkheid van het rollen van een 1. Hoe minder kanten, hoe meer kans je hebt om 1 te gooien en hoe meer zijden hoe minder waarschijnlijk. Met een 4-zijdige dobbelstenen bijvoorbeeld, is er een kans van één op 4 om een 1 te gooien en met een 20 zijdige dobbelsteen er is één kans op de 20. Gelukkig heeft Sonic Pi de handige 'one_in' fn om precies dit te bepalen. Laten we gaan gooien:

```
live_loop :different_probabilities do
  sample :drum_snare_hard if one_in(6)
  sleep 0.125
end
```

Start de bovenstaande live-loop en je hoort het bekende random ritme. Stop deze nog even niet maar wijzig de `6` naar een andere waarde, zoals `2` of `20` en druk op de `Afspeel` - knop. Merk op dat een lager getal betekent dat je de snare drum vaker hoort en dat hogere cijfers de snare minder zullen triggeren. Je maakt nu muziek met kansberekening!

## Probabiliteit combineren

Echt spannend wordt het wanneer je meerdere samples combineert, die worden geactiveerd met verschillende kansen. Bijvoorbeeld:

```
live_loop :multi_beat do
  sample :elec_hi_snare if one_in(6)
  sample :drum_cymbal_closed if one_in(2)
  sample :drum_cymbal_pedal if one_in(3)
  sample :bd_haus if one_in(4)
  sleep 0.125
end
```

Opnieuw, laat de bovenstaande code lopen, en wijzig de probabiliteit, om het ritme te modificeren. Probeer het ook de samples te veranderen om een geheel nieuw gevoel te creëren. Probeer bijvoorbeeld `:drum_cymbal_closed` naar `:bass_hit_c` te veranderen voor extra bass!


## Herhaalbare ritmes

Vervolgens kunnen we gebruik maken van onze oude vriend `use_random_seed` om de willekeurige stroom, na 8 herhalingen opnieuw in te stellen om een regelmatige beat te maken. Typ de volgende code om een meer regelmatig en herhalend ritme te horen. Zodra je de beat hoort, probeer de seed waarde van `1000` naar een ander nummer te wijzigen. Merk op hoe verschillende cijfers, verschillende beats genereren.

```
live_loop :multi_beat do
  use_random_seed 1000
  8.times do
    sample :elec_hi_snare if one_in(6)
    sample :drum_cymbal_closed if one_in(2)
    sample :drum_cymbal_pedal if one_in(3)
    sample :bd_haus if one_in(4)
    sleep 0.125
  end
end
```

Wat ik met dit soort structuren ook graag doe is, de seeds, welke goed klinken,te onthouden en deze te noteren. Op die manier kan ik makkelijk mijn favoriete ritmes opnieuw maken in toekomstige trainingen of voorstellingen.

## Waardoor het allemaal samen valt

Tot slot kunnen we er een random baslijn aan toevoegen om er een mooie melodische inhoud aan te geven. Merk ook op dat we onze pas aangeleerde probabilistische sequentie methode, kunnen gebruiken op synths en samples. Laat het daar zeker ook niet bij, pas de getallen aan, en maak je eigen nummer met de kracht van waarschijnlijkheden!

```
live_loop :multi_beat do
  use_random_seed 2000
  8.times do
    c = rrand(70, 130)
    n = (scale :e1, :minor_pentatonic).take(3).choose
    synth :tb303, note: n, release: 0.1, cutoff: c if rand < 0.9
    sample :elec_hi_snare if one_in(6)
    sample :drum_cymbal_closed if one_in(2)
    sample :drum_cymbal_pedal if one_in(3)
    sample :bd_haus, amp: 1.5 if one_in(4)
    sleep 0.125
  end
end
```
