A.2 Live codering

# Live Coderen

De laserstralen sneden door de slierten rook, terwijl de subwoofer de bassen diep in de lichamen van de menigte pompten. De sfeer was rijp met een onstuimige mix van synths en dansbewegingen. Maar tegelijkertijd klopte er iets niet in deze discotheek. In felle kleuren boven de DJ booth was een futuristische tekst geprojecteerd die knipperde en heen en weer bewoog. Het waren geen hype visuals, maar een projectie van Sonic Pi op een Raspberry Pi. De aanwezige in de DJ booth draaide geen plaatjes, hij was code aan het schrijven, aanpassen en redigeren. Live. Dit is Live Coding.

![Sam Aaron Live Coding](../../../etc/doc/images/tutorial/articles/A.02-live-coding/sam-aaron-live-coding.png)

Dit klinkt misschien als een vergezocht verhaal van een futuristische nachtclub maar codering van muziek zoals deze is een groeiende trend en wordt vaak omschreven als Live codering (http://toplap.org). Een van de recente benadering van muziek spelen is de Algorave (http://algorave.com) - evenementen waar artiesten zoals ik code schrijven voor mensen om op muziek te dansen . Echter, je hoeft niet naar een nachtclub te trekken voor een Live Code - met Sonic Pi v2.6 + kan je het overal waar je je Raspberry Pi kunt meenemen met een paar hoofdtelefoons of luidsprekers. Voor je het einde van dit artikel kan lezen, zal je je eigen beats programmeren en hen live wijzigen. Waar je enkel beperkt wordt door uw verbeelding.

## Live Loop

De hoofdzaak bij het live coderen ligt bij het beheersen van de `live_loop`. Hier kijken we naar zo ééntje:

```
live_loop :beats do
  sample :bd_haus
  sleep 0.5
end
```

Er zitten 4 basisingrediënten in een `live_loop`. De eerst is de naam. Onze `live_loop`heet `:beats`. Je kan jouw `live_loop`benoemen zoals jij dat wil. Doe dwaas. Wees creatief. Ikzelf gebruik benamingen die een uitleg geven over welke klank zij naar het publiek brengen. Het volgende ingrediënt is het `do` woord die markeert waar de `live_loop` start. het derde is het `end`woord dat het einde van onze`live_loop markeert, en tenslotte heb je de inhoud van de `live_loop` waarin beschreven wordt wat de loop gat herhalen- dat is het stukje tussen `do` en `end`. In dit geval gaan we de bass drum sample met een halve tel rusttijd spelen . Dit produceert een aangenaam regelmatige bass beat. Ga je gang, kopieer deze in een lege buffer en klik op afspelen. Boem, boem, boem!.

## Ter plekke herdefiniëren

OK, dus wat is er nu zo speciaal aan de 'live_loop'? Tot nu toe lijkt het gewoon een veredelde 'loop'! Nou, de schoonheid van ' de live_loop is dat u hen ter plekke kan herdefiniëren. Dit betekent dat terwijl deze nog steeds speelt, kan je veranderen wat ze doen. Dit is het geheim van het live coderen met Sonic Pi. Laten we eens spelen:

```
live_loop :choral_drone do
  sample :ambi_choir, rate: 0.4
  sleep 1
end
```

Klik nu op de afspeelknop of gebruik `alt-r`. Je luistert nu naar de prachtige klank van een koor. Verander de koers, terwijl de code speelt van `0.4` naar `0.38`. Klik terug op afspelen. Wauw! Hoorde je het koor van toon veranderen? Verander deze terug naar `0.4` om terug te keren hoe het was. Verlaag deze nu naar `0.2`, verlaag naar `0.19` en terug naar omhoog naar `0.4`. Hierbij zie je dat het improviseren op één parameter, jou de controle over jouw muziek geeft? Speel zelf met de waarden die je kiest. Probeer negatieve getallen, hele kleine en hele grote getallen. Experimenteer!

## Slapen is belangrijk

Een van de belangrijkste lessen over `live_loop`s is dat ze rust nodig hebben. Beschouw deze volgende 'live_loop':

```
live_loop :infinite_impossibilities do
  sample :ambi_choir
end
```

Wanneer je deze code probeert te laten lopen, zie je onmiddellijk dat Sonic Pi klaagt dat de `live_loop` geen `sleep` toebedeeld heeft gekregen. Dit is het veiligheidssysteem dat in werking treedt! Nemen even de tijd om erover na te denken wat je aan de computer vraagt met deze code. Juist, het vraagt de computer een oneindig aantal koorsamples te af te spelen op nul tijd. Zonder het veiligheidssysteem zou de arme computer crashen tijdens dit proces. Dus onthou dat jouw `live_loop` een `sleep` moet bevatten.


## Geluiden combineren

In muziek gebeuren veel dingen tezelfdertijd. Drums spelen op het zelfde moment als de bass en op het zelfde moment als zang en gitaar... In Computing noemen we dit samenwerking en Sonic Pi biedt ons een ongelofelijk eenvoudige manier om zaken samen te laten spelen. Gewoon gebruik maken van verschillende `live_loop`s!

```
live_loop :beats do
  sample :bd_tek
  with_fx :echo, phase: 0.125, mix: 0.4 do
    sample  :drum_cymbal_soft, sustain: 0, release: 0.1
    sleep 0.5
  end
end
live_loop :bass do
  use_synth :tb303
  synth :tb303, note: :e1, release: 4, cutoff: 120, cutoff_attack: 1
  sleep 4
end
```

Hier hebben we twee `live_loops`, de ene speelt snel en maakt beats, de andere speelt snel en maakt maffe bas geluiden.

Eén van de interessante zaken aan het gebruik van meerdere `live_loop`s is dat deze hun eigen onafhankelijke tijd kunnen hebben. Dit betekend dat je makkelijk interessante poly-ritmische structuren kan maken en zelfs kan spelen met fase-verschuivingen in de stijl van Steve Reich. Check deze:

```
# Steve Reich's Piano Phase
notes = (ring :E4, :Fs4, :B4, :Cs5, :D5, :Fs4, :E4, :Cs5, :B4, :Fs4, :D5, :Cs5)
live_loop :slow do
  play notes.tick, release: 0.1
  sleep 0.3
end
live_loop :faster do
  play notes.tick, release: 0.1
  sleep 0.295
end
```

## Waardoor het allemaal samen valt

In elk van deze tutorials eindigen we met een muzikaal voorbeeld waarin alle ideeën die werden voorgesteld samen worden gebracht. Bekijk de code en probeer je voor te stellen wat deze zou gaan doen. Plak deze dan in een lege buffer in Sonic Pi en druk op afspelen, en luister nu hoe dit klinkt. Verander tenslotte één van de getallen en schakel commentaarlijnen in om lijnen in en uit te schakelen. Bekijk of je dit ook niet kan gaan gebruiken in een live optreden, en boven alles,beleef hier plezier aan! Tot volgende keer...

```
with_fx :reverb, room: 1 do
  live_loop :time do
    synth :prophet, release: 8, note: :e1, cutoff: 90, amp: 3
    sleep 8
  end
end
live_loop :machine do
  sample :loop_garzul, rate: 0.5, finish: 0.25
  sample :loop_industrial, beat_stretch: 4, amp: 1
  sleep 4
end
live_loop :kik do
  sample :bd_haus, amp: 2
  sleep 0.5
end
with_fx :echo do
  live_loop :vortex do
    # use_random_seed 800
    notes = (scale :e3, :minor_pentatonic, num_octaves: 3)
    16.times do
      play notes.choose, release: 0.1, amp: 1.5
      sleep 0.125
    end
  end
end
```
