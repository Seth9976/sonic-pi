A.3 Gecodeerd Beats

# Gecodeerd Beats

Een van de meest revolutionaire en vernieuwende ontwikkeling op technisch gebied was de uitvinding van de sampler.Dit waren kisten die jou toelieten eender welke klank op te nemen, te manipuleren en terug af te spelen, op vele interessante manieren. Bijvoorbeeld kon je van een oude plaat een drumsolo (of een break) nemen, deze opnemen op jouw sampler en deze dan op repeat aan halve snelheid afspelen om de basis te leggen van je nieuwe beats. Dit is hoe old school hip-hop werd geboren en vandaag is het haast onmogelijk om Elektronische muziek te vinden die geen gebruik maken van één of andere sample. Het gebruik van samples is een geweldige manier om makkelijk nieuwe en interessante elementen aan jouw live code toe te voegen.

Dus waar kan je zo'n sampler krijgen? Wel je hebt er al één- het is jouw Raspberry Pi! The ingebouwde codeer app Sonic Pi heeft een uiterst krachtige sampler ingebouwd in zijn core. Laten we ermee gaan spelen!

## De Amen Break

Een van de meest klassiek en herkenbare drum break samples is de Amen Break. Het werd voor het eerst uitgevoerd in 1969 in het nummer "Amen Brother" door de Winstons als een gedeelte in een drumbreak. Het was echter wanneer het werd ontdekt door de vroege hip-hop muzikanten in de jaren 80 en werd gebruikt in samplers dat de start werd gegeven deze sample in vele stijlen te gaan gebruiken zoals drum& bass, breakbeat, hardcore techno en breakcore.

Ik ben er zeker van dat je blij bent dat deze standaard ingebouwd zit in Sonic Pi. Ruim even een buffer leeg en gooi er de volgende code in:

```
sample :loop_amen
```

Klik op *Afspelen* en Boem. Je luistert nu naar één van de meest invloedrijke drumbreaks in de geschiedenis van dance music. Als one-shot was deze niet populair natuurlijk wel als loop.


## Beat Stretching

Laat ons de Amen Break loopen in onze goede vriend `live_loop`, geïntroduceerd in deze tutorial van vorige maand:

```
live_loop :amen_break do
  sample :loop_amen
  sleep 2
end
```

OK, deze loopt nu, maar er is wel een vervelende pauze elke keer dat hij zijn cyclus heeft volbracht. Dat komt omdat we het een `sleep`-tijd van`2`tellen hebben gegeven en met de standaard 60 BPM duurt de `:loop_amen` sample `1.753` tellen. Hierdoor hebben we een stilte van `2 - 1.753 = 0.247` tellen. Ook al is deze kort, het is wel merkbaar.

Om dit probleem op te lossen kunnen we de `beat_stretch`opt gebruiken om Sonic Pi op te dragen deze sample uit te rekken (of te doen inkrimpen) om met het opgegeven aantal tellen overeen te laten komen.

De `sample`en `synth` fns van Sonic Pi geven jou veel controle over de optionele parameters zoals `amp:`, `cutoff:`en `release:` . Maar de term optionele parameter is wel een mond vol, dus noemen we deze *opts* om de zaken eenvoudig te houden.

```
live_loop :amen_break do
  sample :loop_amen, beat_stretch: 2
  sleep 2
end  
```

Nu zijn we aan het dansen! Maar misschien willen we het wat sneller of wat trager om de juiste stemming te pakken te krijgen.

## Spelen met Tijd

Ok, en als we de stijl willen aanpassen naar old school hip hop of breakcore? Een eenvoudige manier om dit te gaan doen is te spelen met tijd -of in andere woorden: aan het tempo klooien. Dit is makkelijk te doen in Sonic Pi, gooi gewoon `use_bpm` in je live loop:

```
live_loop :amen_break do
  use_bpm 30
  sample :loop_amen, beat_stretch: 2
  sleep 2
end 
```

Terwijl je over deze trage beats heen rapt merk je dat we nog steeds voor 2 `sleep`hebben en dat onze BPM 30 is, maar alles is wel in maat. De `beat_stretch`opt werkt met de huidige BPM om ervoor te zorgen dat alles werkt.

Nu komt het leuke! Terwijl de loop nog altijd speelt, verander de`30`in de `use_bpm 30`regel naar `50`. Alles gaat nu sneller, en wordt nog steeds *in maat* gehouden, goed hoor! Probeer sneller te gaan - omhoog naar 80, naar 120, doe nu eens goed zot en gebruik 200!


## Filteren

Nu we samples kunnen live lopen, laat ons eens kijken naar de leukste opts die bij de `sample`synth geleverd zijn. Eerst komt `cutoff`aan beurt, welke de cutoff op de filter van de sampler regelt. Standaard is deze uitgeschakeld, maar je kan deze makkelijk aanzetten:

```
live_loop :amen_break do
  use_bpm 50
  sample :loop_amen, beat_stretch: 2, cutoff: 70
  sleep 2
end  
```

Ga je gang en verander de `cutoff:` opt. Verhoog deze bijvoorbeeld naar 100, klik op *Afspelen* en wacht een cyclus van de loop af om de verandering van het geluid waar te nemen. Merk op dat lage waarden zoals 50 zacht klinken en laag en hoger waarden zoals 100 en 120 voller klinken en rasperig. Dit is omdat de `cutoff:`opt de hoge geluidsfrequentie afsnijdt zoals een grasmaaier de grasuiteinden afknipt. De `cutoff:` opt is zoals de lengte instelling, en bepaald hoeveel gras overblijft.


## Slicing (Snijden)

Nog een fantastisch hulpmiddel om mee aan de slag te gaan is de slicer FX. Deze zal het geluid uiteen hakken (snijden). Omhul de `sample`regel met de FX code als volgt:

```
live_loop :amen_break do
  use_bpm 50
  with_fx :slicer, phase: 0.25, wave: 0, mix: 1 do
    sample :loop_amen, beat_stretch: 2, cutoff: 100
  end
  sleep 2
end
```

Merk op hoe de klank op en neer stuitert, elke keer meer. (Je kan de originele klank, zonder FX, beluisteren door de `mix` opt naar `0`te veranderen) Speel nu met de `phase:` opt. Dit is de snelheid (in slagen) van het slicing effect. Een kleinere waarde zoals `0.125` zal sneller hakken en grotere waarden zoals `0.5` zullen trager snijden. Merk hierbij op dat het opeenvolgend halveren of verdubbelen van de `phase:`opt waarde altijd goed schijnt te klinken. Verander tenslotte de `wave:` opt naar 0,1 of 2 en hoor hoe dit de klank veranderd. Dit zijn de verschillende golfvormen. 0 is een zaagtand-golf, (harde aanslag, fade-out) 1 is een blokgolf (harde aanslag, geen fade-out) en 2 is een driehoeksgolf (fade-in, fade out).


## Waardoor het allemaal samen valt

Laten we tenslotte terug in de tijd gaan en de vroege drum&bass scene van Bristol bezoeken, met het voorbeeld van de maand. Maak je geen zorgen als je echt niet weet over wat dit gaat, type deze even, klik op Afspelen, begin te live code-en, door de getallen van de opt te veranderen en kijk wat je dit oplevert. Deel alsjeblieft wat je hebt gemaakt! Tot Volgende keer...

```
use_bpm 100
live_loop :amen_break do
  p = [0.125, 0.25, 0.5].choose
  with_fx :slicer, phase: p, wave: 0, mix: rrand(0.7, 1) do
    r = [1, 1, 1, -1].choose
    sample :loop_amen, beat_stretch: 2, rate: r, amp: 2
  end
  sleep 2
end
live_loop :bass_drum do
  sample :bd_haus, cutoff: 70, amp: 1.5
  sleep 0.5
end
live_loop :landing do
  bass_line = (knit :e1, 3, [:c1, :c2].choose, 1)
  with_fx :slicer, phase: [0.25, 0.5].choose, invert_wave: 1, wave: 0 do
    s = synth :square, note: bass_line.tick, sustain: 4, cutoff: 60
    control s, cutoff_slide: 4, cutoff: 120
  end
  sleep 4
end
```
