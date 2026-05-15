A.14 Amplitude Modulatie

# Amplitude Modulatie

Deze maand gaan we een diepe duik nemen in een van Sonic Pi ' s meest krachtige en flexibele audio effecten: de `: slicer`. Aan het eind van dit artikel heb je geleerd hoe je de totale omvang van de onderdelen van ons live gecodeerd geluid op een krachtige, nieuwe manieren kan manipuleren. Dit zal je toelaten, nieuwe ritmische en timbrale structuren te creëren en je klankspectrum te verruimen.

## Snij de Amp

Dus, wat doet deze:slicer` FX eigenlijk? Een manier om er naar te kijken is ,dat het net iemand is, die speelt met de volumeknop op de stereoinstallatie of de TV. Laten we eens een kijkje nemen, maar zeker eerst luisteren naar het diepe gegrom van de volgende code die de `:prophet` synth triggeren:

```
synth :prophet, note: :e1, release: 8, cutoff: 70
synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
```

Laat ons dit nu door het `:slicer`FX leiden:

```

with_fx :slicer do
  synth :prophet, note: :e1, release: 8, cutoff: 70
  synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
end
```

Hoor hier, hoe de Slicer, het geluid met een een regelmatige tel, lijkt op en aan te zetten.Merk hierbij op dat de `:slicer` het geluid, gegenereerd tussen de `do`/`end` blocks, beïnvloed. Je kan de snelheid waarmee het geluid wordt af- en aangezet bepalen met de `phase:` opt ( duurtijd fase). Zijn standaard instelling is `0.25`, dat is 4 keer per minuut aan de standaard 60 BPM. Dit maken we sneller:

```
with_fx :slicer, phase: 0.125 do
  synth :prophet, note: :e1, release: 8, cutoff: 70
  synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
end
```

Speel nu zelf met de verschillende `phase:` tijden. Probeer langere en kortere waarden. Zie wat er gebeurt wanneer je kiest voor een heel korte-waarde. Probeer ook verschillende synths zoals `:beep` of `:dsaw` en probeer ook andere noten. Bekijk het volgende diagram om te zien hoe verschillende `phase:` waarden, het aantal veranderingen op de amplitude per beat wijzigt.

![Phase Durations](../../../etc/doc/images/tutorial/articles/A.14-amplitude-modulation/slicer_phase_durations.png)

De duur van een fase is de lengte van een aan/uit cyclus. Daarom zullen kleinere waarden, de FX schakelaar veel sneller aan-/ uitschakelen, dan grotere waarden. Leuke waarden om mee te beginnen spelen zijn `0.125`, `0.25`, `0.5` en `1`.


## Golfsoort Instellen

Standaard gebruikt het `:slicer` FX een blokgolf om de amplitude in een bepaalde tijdsduur te manipuleren. Daarom horen we amplitude voor een tijdje, en dan voor een tijdje helemaal niet, om dan weer met een zelfde cyclus te beginnen. Het is zo dat de blokgolf één van de vier golfvormen is die we kunnen instellen in deze `:slicer`. De andere golfsoorten zijn: zaagtand, driehoeksgolf, en (co)sinus. Kijk ook eens naar de onderstaande afbeelding om te zien hoe deze eruit zien. We kunnen ook horen hoe deze klinken. De volgende code gebruikt een sinusgolf als controlegolf. Hoor nu dat het geluid niet abrupt aan en uit gaat, maar zacht in- en uit-fade:

```
with_fx :slicer, phase: 0.5, wave: 3 do
  synth :dsaw, note: :e3, release: 8, cutoff: 120
  synth :dsaw, note: :e2, release: 8, cutoff: 100
end
```

Speel met verschillende golftypes door de `wave:` opt naar `0` te veranderen voor saw (zaagtandgolf) , `1` voor square (blokgolf), `2` voor triangle (driehoeksgolf) en `3` voor sine (sinusgolf). Bekijk ook hoe deze verschillende golftypes klinken met verschillende `phase:` opts.

Elk van deze golven kan worden omgekeerd met de `invert_wave:` optie, en spiegelt deze tegenover de Y-as. Bijvoorbeeld, in een enkele fase, zal een zaagtand-golf normaal gezien op zijn hoogste punt beginnen en langzaam naar beneden te gaan, om dan terug naar zin hoogste punt te springen. Met `invert_wave: 1` zal deze nu op het laagste punt beginnen en langzaam naar boven gaan, om dan naar beneden te vallen. Bijkomend, kan je de golf ook instellen dat deze op andere punten start met de `phase_offset:` optie. Deze kan een getal bevatten tussen `0` en`1`. Door te gaan spelen met de `phase:`, `wave:`, `invert_wave:` en`phase_offset` opties kan je de amplitude binnen een tijdsverloop dramatisch veranderen.

![Phase Durations](../../../etc/doc/images/tutorial/articles/A.14-amplitude-modulation/slicer_control_waves.png)


## Het instellen van je niveaus

Standaard schakelt `:slicer` tussen de amplitude-waarden `1` (luid) en `0` (stil). Dit kan je veranderen met de `amp_min:` en `amp_max:` opties. Je kan deze gebruiken naast de sinusgolf instelling om een eenvoudige tremolo-effect te bekomen:

```
with_fx :slicer, amp_min: 0.25, amp_max: 0.75, wave: 3, phase: 0.25 do
  synth :saw, release: 8
end
```

Dit is zoals je de volumeknop van je hi-fi op en neer zou bewegen, zodat het geluid een beetje aan en uit 'wiebelt'.


## Probabiliteit

Een van de krachtigste functies van ': slicer' , is de mogelijkheid of u de slicer aan of uit wil schakelen. Vooraleer het `:slicer` FX een nieuwe fase start, zal deze een teerling werpen, en gebaseerd op de uitkomst hiervan , de geselecteerde controle golf gebruiken, of de amplitude niet aanzetten. Laten we even gaan luisteren:

```
with_fx :slicer, phase: 0.125, probability: 0.6  do
  synth :tb303, note: :e1, cutoff_attack: 8, release: 8
  synth :tb303, note: :e2, cutoff_attack: 4, release: 8
  synth :tb303, note: :e3, cutoff_attack: 2, release: 8
end
```

Hoor hoe we nu een interessant ritme van pulsen hebben bekomen. Probeer de `probability:` optie naar een andere waarde tussen `0` en `1` te veranderen. Waarden dichter bij `0` zullen meer ruimte tussen elk geluid hebben, doordat de waarschijnlijkheid dat het geluid wordt getriggerd veel lager ligt.

Wat je zeker ook nog moet weten is, dat het probabiliteitssysteem in FX, net zoals het randomisatie-systeem toegankelijk is via functies zoals `rand` en`shuffle`. Ze zijn beide volledig deterministisch. Dit betekent dat elke keer je op afspelen klikt, je precies hetzelfde ritme van pulsen zal horen binnen een bepaalde probabiliteit. Wil je toch nog dingen veranderen, gebruik dan de `seed:` opt om een verschillend start seed te kiezen. Dit werkt precies hetzelfde als `use_random_seed` maar zal enkel invloed hebben op dat bepaalde FX.

Tenslotte kan je ook nog de 'rust' positie van de controle-Golf als de probabiliteit-test mislukt, van `0` naar eender welke positie brengen met de `prob_pos:` opt:

```
with_fx :slicer, phase: 0.125, probability: 0.6, prob_pos: 1  do
  synth :tb303, note: :e1, cutoff_attack: 8, release: 8
  synth :tb303, note: :e2, cutoff_attack: 4, release: 8
  synth :tb303, note: :e3, cutoff_attack: 2, release: 8
end
```

## Beats Slicen (Opknippen)

Wat ook heel leuk is om te doen is het gebruik van `:slicer` om een drum beat aan en uit te knipperen:

```
with_fx :slicer, phase: 0.125 do
  sample :loop_mika
end
```

Hiermee kunnen we met een willekeurige sample nieuwe ritmische mogelijkheden creëren. Dit is heel leuk om mee te spelen. Waar je wel op moet letten is dat het tempo van de sample past in het huidige BPM van Sonic Pi, anders knipt hij naast het ritme. Probeer bijvoorbeeld de `:loop_mika` met de `loop_amen` sample te verwisselen, om te horen hoe slecht dit kan klinken als het tempo niet goed overeenkomt.

## Tempo Veranderen

Zoals we reeds gezien hebben, als het standaard BPM met `use_bpm` wordt veranderd, zal dit ervoor zorgen dat de rusttijden (sleep) en de enveloppe van de synths gaan groeien of krimpen, om gelijk met de tel te kunnen lopen. De `:slicer` FX houdt hier ook rekening mee, vermits de `phase:` optie in de maat wordt berekend en niet in seconden. We kunnen dus bovenstaand probleem oplossen door de BPM te veranderen om deze met de sample te laten passen:

```
use_sample_bpm :loop_amen
with_fx :slicer, phase: 0.125 do
  sample :loop_amen
end
```

## Waardoor het allemaal samen valt

Laten we deze ideeën nu even toepassen in een laatste voorbeeld waar enkel het `:slicer` FX wordt gebruikt om een interessante combinatie te maken. Ga je gang, begin dingen te veranderen en maak er je eigen nummer van!

```
live_loop :dark_mist do
  co = (line 70, 130, steps: 8).tick
  with_fx :slicer, probability: 0.7, prob_pos: 1 do
    synth :prophet, note: :e1, release: 8, cutoff: co
  end
  
  with_fx :slicer, phase: [0.125, 0.25].choose do
    sample :guit_em9, rate: 0.5
  end
  sleep 8
end
live_loop :crashing_waves do
  with_fx :slicer, wave: 0, phase: 0.25 do
    sample :loop_mika, rate: 0.5
  end
  sleep 16
end
```




