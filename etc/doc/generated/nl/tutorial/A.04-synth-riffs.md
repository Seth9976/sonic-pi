A.4 Synth Riffs

# Synth Riffs

Of het nu spokende onstuimige, grommende oscillatoren zijn of de ontstemde aanslag van zaagtand-golven die door de mix klieven, de lead synth zal altijd een essentiële rol spelen in een elektronisch nummer. In de editie van vorige maand in deze serie zelf-studies zagen we hoe je je beats code maakt. In deze tutorial gaan we zien hoe we de code doen van drie hoofd componenten van een synth riff: het timbre, de melodie en het ritme.

Ok, zet je Raspberry Pi op, maak Sonic Pi v2.6+ open en maak wat lawaai!


## Timbrale mogelijkheden

Een essentieel onderdeel van zowat elke synth riff ligt in het veranderen en bespelen van het timbre. We kunnen het timbre in Sonic Pi op twee verschillende manieren controllen: veranderen van synth voor een drastische verandering en de verschillende synth opts gebruiken voor een meer subtiele aanpak. We kunnen ook FX gebruiken, maar dat is voor een andere tutorial...

We creëren een eenvoudige loop waar we de huidige synth voortdurend gaan veranderen:

```
live_loop :timbre do
  use_synth (ring :tb303, :blade, :prophet, :saw, :beep, :tri).tick
  play :e2, attack: 0, release: 0.5, cutoff: 100
  sleep 0.5
end
```

Kijk even naar de code. We tikken gewoon door een ring van synth namen (deze zal de lijst helemaal doorlopen en aan het einde opnieuw beginnen). We geven deze synth naam door aan de `use_synth` fn (functie) die de `live_loop`'s huidige synth verandert. We spelen ook de noot `:e2` (E van het tweede octaaf), met een release tijd van 0.5 tel (een halve seconde aan de standaard BPM van 60) en met een `cutoff:`opt naar 100 gezet.

Hoor hoe de verschillende synths zeer grote verschillen vertonen hoewel ze dezelfde noot spelen. Experimenteer nu en speel hiermee. Verander de release tijden naar grotere en kleinere waarden. Verander bijvoorbeeld de `attack:` en `release:` opts om te zien hoe verschillende cutoff waarden een invloed hebben op het timbre (waarden tussen 60 en 130 zijn goed) Kijk hoeveel verschillende klanken je kan creëren door simpelweg enkele waarden aan te passen. Eens je dit onder de knie hebt, ga dan naar de synths tab in het hulpsysteem voor de lijst van alle synths en de voor iedere individuele synth beschikbare opts, om te zien hoeveel kracht je hiermee onder je codende vingers hebt.

## Timbre

Timbre of klankkleur is enkel een moeilijk woord om de klank van een geluid te beschrijven. Als je dezelfde noot speelt met verschillende instrumenten zoals viool, gitaar of piano zal de toonhoogte altijd dezelfde zijn, maar de klankeigenschap zal anders zijn. Deze eigenschap die het mogelijk maakt om het verschil tussen een piano en een gitaar waar te nemen is het timbre.


## Melodische Compositie

Een ander belangrijk aspect dat onze lead synth is de keuze van de noten die wij willen spelen. Als je al een goed idee hebt, dan kan je gewoon een ring met jouw noten :

```
live_loop :riff do
  use_synth :prophet
  riff = (ring :e3, :e3, :r, :g3, :r, :r, :r, :a3)
  play riff.tick, release: 0.5, cutoff: 80
  sleep 0.25
end
```
    
Hier hebben de melodie gedefinieerd met een ring die beide noten `:e3` en de rust ´:r bevat. We gebruiken dan `.tick` om door elke noot te lopen, om zo een herhalende riff te krijgen.

## Auto Melodie

Het is niet altijd gemakkelijk om met een mooie riff voor de dag te komen. In plaats daarvan is het vaak makkelijker Sonic Pi te vragen naar een selectie van willekeurig riff's en waaruit je dan kan kiezen ,welke jou het beste bevalt. Om dit te kunnen gaan doen, moeten we drie zaken combineren: ringen, randomiseren en random seeds. Laten we eens kijken naar een voorbeeld:

```
live_loop :random_riff do
  use_synth :dsaw
  use_random_seed 3
  notes = (scale :e3, :minor_pentatonic).shuffle
  play notes.tick, release: 0.25, cutoff: 80
  sleep 0.25
end
```

Een aantal zaken gebeuren hier, laat ons deze even afzonderlijk bekijken. Eerst specifiëren we dat we random seed 3 gebruiken. Wat betekent dit? Wel het nuttige hieraan is dat wanneer we de seed instellen, we ook kunnen voorspellen wat de volgende willekeurige waarde zal zijn, namelijk dezelfde als de laatste keer dat we de seed op 3 hadden ingesteld! Goed om weten is ook, dat een shuffle van kring van noten, op dezelfde manier werkt. In het bovenstaande voorbeeld vragen we eigenlijk de 'derde shuffle' in de standaard lijst van shuffles op, die steeds ook dezelfde is, gezien we de random seed instellen op de zelfde waarde vlak voor de shuffle. Ten slotte tikken we met `.tick` door de ge-shuffle-de noten, om onze riff af te spelen.

En nu wordt het leuk. Als we de waarde van de random seed veranderen naar bvb. 3000, krijgen we een geheel nieuwe shuffle van noten. Het is nu wel extreem makkelijk om nieuwe melodieën te gaan ontdekken. Kies simpelweg de lijst van de noten die je wil verschuiven met shuffle ( toonladders zijn hierbij een mooi uitgangspunt) en kies dan de seed waarmee je de shuffle wil doen. Als de melodie jou niet bevalt, verander dan één van deze twee dingen en probeer opnieuw. Herhaal dit tot je iets tegenkomt dat je wel graag hoort!


## Pseudo randomiseren

Sonic Pi's randomiseren is niet echt willekeurig (random) en is daarom pseudo random. Stel je voor je een dobbelsteen 100 keer zou opgooien en het resultaat van elke opgooi op een stuk papier zou moeten noteren. Sonic Pi heeft het equivalent van deze resultatenlijst, die zij dan gebruikt, wanneer je om een willekeurige waarde vraagt. In plaats van werkelijke dobbelstenen, pakt het gewoon de volgende waarde uit de lijst. Het instellen van random seed, is gewoon het springen naar een bepaald punt in die lijst.
 
## Jouw Ritme Vinden

Nog een belangrijk element van onze riff is het ritme, wanneer een noot spelen en wanneer niet. Zoals we hierboven al zagen, kunnen we `:r` in onze ringen gebruiken, om een rusttijd in te zetten. Nog een andere, krachtige, manier is het gebruik maken van spreads, wat we in een toekomstige leeroefening gaan bekijken. Vandaag zullen we gebruik maken van randomiseren, om ons te helpen, ons ritme te vinden. In plaats van elke noot te spelen kunnen we een voorwaardelijke gebruiken, om een noot te spelen met een gegeven waarschijnlijkheid. Laat ons eens een kijkje nemen:

```
live_loop :random_riff do
  use_synth :dsaw
  use_random_seed 30
  notes = (scale :e3, :minor_pentatonic).shuffle
  16.times do
    play notes.tick, release: 0.2, cutoff: 90 if one_in(2)
    sleep 0.125
  end
end
```

Een heel nuttige functie om te onthouden is `one_in`, welke ons een `true` of `false` waarde geeft met een bepaalde waarschijnlijkheid. Hier gebruiken we een waarde van 2, die gemiddeld één keer per twee oproepen naar `one_in`, `true zal weergeven. Door hogere waarden te gebruiken kunnen we `false` meer weergegeven laten worden, waardoor we meer ruimte in de riff kunnen creëren.

Merk op dat we hier een iteratie met `16.times` hebben toegevoegd. Dit doen we alleen omdat we onze random seed waarde om de 16 noten willen resetten, zodat ons ritme elke 16 keer herhaalt. Dit beïnvloed de shuffle niet, vermits deze direct achter de seed staat. We kunnen de grootte van de iteratie wijzigen om, om de lengte van de riff te veranderen. Probeer 16 naar 8 te veranderen of zelfs aar 4 of 3 en zie hoe dit de lengte van de riff beïnvloed.

## Waardoor het allemaal samen valt

Ok, laat ons nu alles wat we geleerd hebben in een laatste voorbeeld combineren. Tot volgende keer!

```
live_loop :random_riff do
  #  uncomment to bring in:
  #  synth :blade, note: :e4, release: 4, cutoff: 100, amp: 1.5
  use_synth :dsaw
  use_random_seed 43
  notes = (scale :e3, :minor_pentatonic, num_octaves: 2).shuffle.take(8)
  8.times do
    play notes.tick, release: rand(0.5), cutoff: rrand(60, 130) if one_in(2)
    sleep 0.125
  end
end
 
live_loop :drums do
  use_random_seed 500
  16.times do
    sample :bd_haus, rate: 2, cutoff: 110 if rand < 0.35
    sleep 0.125
  end
end
 
live_loop :bd do
  sample :bd_haus, cutoff: 100, amp: 3
  sleep 0.5
end
```
