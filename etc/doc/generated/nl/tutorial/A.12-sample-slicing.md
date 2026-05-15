A.12 Sample Snijden

# Het snijden van een sample

Een hele tijd terug in aflevering 3 van deze Sonic Pi serie, keken we hoe we één van de meest bekende drum breaks aller tijden konden loop-en, stretchen en filteren: de Amen Break. In deze tutorial gaan we nog een stapje verder en gaan we zien hoe we de sample kunnen opknippen, de stukken door elkaar te schudden en deze in een andere volgorde weer aan elkaar te lijmen. Misschien klinkt dit allemaal wat raar nu, maar maak je geen zorgen, het zal je snel allemaal duidelijk worden, en je zal deze techniek snel meester zijn en deze in de toekomst gebruiken in je live gecode set .

## Geluid als data

Voor we eraan beginnen, kijken we even hoe we het gebruik van sample's beter kunnen begrijpen. Ondertussen hebben jullie hopelijk al gespeeld met de krachtige sampler van Sonic Pi. Indien niet, start Raspberry Pi, laad Sonic Pi op, en typ het volgende in een lege buffer om de drum-sample te kunnen horen:

```
sample :loop_amen
```

Een opgenomen geluid wordt simpelweg uitgedrukt in data, veel nummers tussen -1 en 1 zijn dat die de pieken en dalen van een geluidsgolf vertegenwoordigen. Als we deze cijfers in volgorde kunnen reproduceren, krijgen we het originele geluid terug. Maar wat houdt ons tegen om deze in een andere volgorde te gaan afspelen en hierbij een nieuw geluid te creëren?

Hoe zijn samples eigenlijk opgenomen? Dat is eigenlijk vrij eenvoudig als u eenmaal de fundamentele fysica van geluid begrijpt. Bij het produceren van een klank,bijvoorbeeld door op een trommel te slaan, reist het geluid door de lucht,op een zelfde manier het wateroppervlak rimpels maakt als je er een steentje in gooit. Als deze golven jouw oren bereiken, zal jouw trommelvlies hierop reageren en zullen deze bewegingen omgezet worden naar de klank die jij dan hoort. Als we een geluid willen opnemen en terug willen afspelen moeten we deze geluidsgolven capteren, opslaan en reproduceren. We kunnen dit bijvoorbeeld met een microfoon, die net als een trommelvlies reageert en heen en weer zal bewegen zodra een geluidsgolf het membraan raakt. De microfoon registreert deze beweging en zet deze om tot een minuscuul elektrisch signaal dat op zijn beurt vele malen per seconde wordt gemeten.Deze metingen worden dan weergegeven als een reeks getallen tussen -1 en 1.

Als we geluid zouden visualiseren zou deze een simpele grafiek zijn van datagegevens met tijdsverloop op de x-as en de bewegingen van de microfoon/luidspreker met waarden tussen -1 en 1 op de y-as. Je kan, bovenaan het diagram, een voorbeeld van zo'n grafiek zien.

## Een deel van een sample afspelen

Dus, hoe kunnen we dan Sonic Pi zo coderen dat deze een sample in een andere volgorde afspeelt? Om een antwoord op deze vraag te kunnen geven moeten we een kijkje nemen naar de `start:` en`finish:` opts voor `sample`. Deze laten ons toe de start en eindposities voor de weergave van een sample te bepalen. De waarden voor deze opts liggen tussen `0` en`1` waarbij `0` staat voor de de start van de sample en `1` het einde ervan is. Dus als we de eerste helft van de Amen Break willen spelen moeten we de `finish:` zetten op `0.5`:

```
sample :loop_amen, finish: 0.5
```

We kunnen ook een `start:` toevoegen om een nog kleiner stukje van de sample te kunnen spelen:

```
sample :loop_amen, start: 0.25, finish: 0.5
```

Voor de lol kunnen we de waarde van `finish:` laten vallen *vòòr* de waarde van `start:` om de selectie achterstevoren te laten afspelen:

```
sample :loop_amen, start: 0.5, finish: 0.25
```

## Het afspelen van een sample herordenen

Nu we weten dat een sample een simpele lijst is van getallen, die we in eender welke volgorde kunnen laten afspelen en we ook weten hoe we een bepaald deel van de sample kunnen afspelen, kunnen we na aan de slag gaan en een sample in de verkeerde volgorde gooien en afspelen.

![Amen Slices](../../../etc/doc/images/tutorial/articles/A.12-sample-slicing/amen_slice.png)

Laten we onze Amen break nemen en deze in 8 gelijke stukken verdelen en de stukken verschuiven. Kijk even op het diagram: bovenaan A) geeft de grafiek van onze originele sample weer. In 8 stukjes geknipt geeft ons B) , merk op dat we elk stukje een andere kleur hebben gegeven om deze te kunnen onderscheiden. Je ziet bovenaan de waarde van start en einde ,van elk stukje. En tenslotte is C) een mogelijke herordening van de stukjes. Deze kunnen we als een nieuwe beat afspelen. Laten we even deze code bekijken om dit te gaan doen:

```
live_loop :beat_slicer do
  slice_idx = rand_i(8)
  slice_size = 0.125
  s = slice_idx * slice_size
  f = s + slice_size
  sample :loop_amen, start: s, finish: f
  sleep sample_duration :loop_amen, start: s, finish: f
end
```

1. We kiezen een willekeurige segment om af te spelen welke een willekeurig getal tussen 0 en 7 is (voor de 8 stukken, we tellen vanaf 0!) Hiervoor heeft Sonic Pi een handige functie: `rand_i(8)`. Vervolgens bewaren we deze willekeurige segment-index in de variabele `slice_idx`.
2. Definiëren we onze `slice_size` die 1/8 is of 0,125 is. De `slice_size` is noodzakelijk om onze 'slice_idx' te converteren naar een waarde tussen 0 en 1 zodat we deze kunnen gebruiken als onze `start:` opt.
3. We berekenen de beginpositie ' door de 'slice_idx' met 'slice_size' te vermenigvuldigen .
4. We berekenen de eind-positie `f` door de ´slice_size` aan de startpositie `s` toe te voegen.
5. We kunnen nu deze sample-slice afspelen door de waarden van `s`en `f`, in de `start:` en `finish:` opts voor `sample` te steken.
6. Voordat we het volgende segment spelen moeten we weten hoe lang de `sleep` is welke de duurtijd is van ons sample segment. Gelukkig heeft Sonic Pi ons voorzien van `sample_duration` welke dezelfde opts heeft als `sample`en ons zijn duurtijd kan weergeven. Dus, door het passeren van `sample_duration` aan onze `start:` en `finish:` kennen we de duur van elke slice.
7. We steken deze code in een `live_loop` zodat we verder nieuwe random slices kunnen uitpikken.


## Waardoor het allemaal samen valt

Laten we alles wat we tot nu toe hebben gezien, met de zelfde aanpak in een laatste voorbeeld combineren, dat demonstreert hoe opgeknipte beats met wat bass het begin van een interessante track kunnen maken . Nu is het jouw beurt - om de onderstaande code als uitgangspunt te nemen en te zien of je deze een eigen twist kan geven om er weer iets helemaal anders van te maken...

```
live_loop :sliced_amen do
  n = 8
  s =  line(0, 1, steps: n).choose
  f = s + (1.0 / n)
  sample :loop_amen, beat_stretch: 2, start: s, finish: f
  sleep 2.0  / n
end
live_loop :acid_bass do
  with_fx :reverb, room: 1, reps: 32, amp: 0.6 do
    tick
    n = (octs :e0, 3).look - (knit 0, 3 * 8, -4, 3 * 8).look
    co = rrand(70, 110)
    synth :beep, note: n + 36, release: 0.1, wave: 0, cutoff: co
    synth :tb303, note: n, release: 0.2, wave: 0, cutoff: co
    sleep (ring 0.125, 0.25).look
  end
end
```
