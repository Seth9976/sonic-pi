A.5 Acid Bass

# Acid Bass

Als we naar de geschiedenis van elektronische dansmuziek kijken, kunnen we niet naast de enorme impact van de kleine Roland TB-303 synthesizer kijken Het is het geheime recept van het originele acid bass geluid.

Interessant om weten is ook dat Roland de TB-303 niet voor dancemuziek heeft ontwikkeld. Het werd oorspronkelijk gemaakt als ondersteuning voor gitaristen bij het oefenen. Ze stelden zich voor dat men bas-lijnen ging programmeren om dan samen met de gitaar te gaan jammen. Helaas waren er een aantal problemen: ze waren een beetje onhandig om te programmeren, niet bijzonder goed als vervanging van een bas-gitaar geluid en vrij duur om te kopen. Het sloeg niet aan en om verlies te vermijden werd er beslist met de productie te stoppen na 10.000 exemplaren. Al snel vonden ze hun weg naar de etalage van de tweedehands winkels. De afgedankte TB-303's lagen nu te wachten om ontdekt te worden door een nieuwe generatie die hen op een manier zou gaan gebruiken die Roland niet voor ogen had. Acid house was geboren.

Vermits het in handen krijgen van een originele TB-303 niet eenvoudig is, zal u blij zijn te horen dat je jouw Raspberry Pi makkelijk kan omtoveren tot een TB-303 met de kracht van Sonic Pi. Ziehier, start Sonic Pi, gooi de code in een lege buffer, en druk op afspelen:

```
use_synth :tb303
play :e1
```
    
Acid Bass in een oogwenk! We gaan hier mee spelen...

## Smak die Bas

Laten we eerst een arpeggiator maken om de zaken leuk te houden. In de laatste tutorial hebben gezien hoe riffs gewoonweg een ring van noten kunnen zijn waar we, de ene na de andere doorheen "tikken", en zich gaan herhalen eens ze aan het einde van de cyclus zijn gekomen. Laten we een live loop maken die dat doet:

```
use_synth :tb303
live_loop :squelch do
  n = (ring :e1, :e2, :e3).tick
  play n, release: 0.125, cutoff: 100, res: 0.8, wave: 0
  sleep 0.125
end
```

Laten we naar elke regel een kijkje nemen.

1. Op de eerste regel zetten we de standaard synth als 'tb303' met de 'use_synth' fn.
2. Op regel twee creëren we een live loop genaamd ': squelch' die in een lus loopt.
3. In regel drie creëren we onze riff, een ring van noten (E in de octaven 1, 2 en 3) die we simpelweg met `.tick` gaan "tikken". We definiëren `n` om de huidige noot in de riff te vertegenwoordigen. Het gelijkheidsteken betekent dat de waarde rechts, aan de naam links wordt verwezen. Deze zal in elke loop cyclus anders zijn. In de eerste loop zal `n` naar `:e1` gezet worden. In de tweede loop cyclus zal dit `:e3`zijn gevolgd door `:e3`, en dan terug naar `:e1`, om zo voor altijd zijn cyclus te herhalen.
4. Regel vier is waar we onze `:tb303` synth activeren. En we passeren aan deze een aantal interessante opts: `release:`, `cutoff:`, `res:` and `wave:` dewelke we hieronder gaan bespreken.
5. Regel vijf is onze ´sleep´' - we vragen de live loop elke ' 0,125` of 8 keer per seconde op de standaard BPM van 60 te gaan loop-en.
6. Regel 6 is het einde van de live loop. Deze vertelt aan Sonic Pi waar het einde van de live loop is.

Terwijl je nog steeds uitzoekt wat er gaande is, typt u de bovenstaande code in en druk op de knop afspelen. Je zou nu de `:tb303` in actie moeten horen schieten. Nu, hier begint de pret: laten we beginnen live-coderen.

Terwijl de loop afspeelt, verander je de `cutoff:` opt naar `110`. Klik nu opnieuw op de Afspeel knop. Je zou het geluid nu harder en rauwer moeten horen afspelen. Verander deze nu op `120` en klik op Afspelen. Nu `130`en hoor het geluid intenser en meer snijdend worden. Ga tenslotte terug naar `80` wanneer het jou ter druk wordt. Herhaal dit zoveel je wil. Geen nood, ik zal hier nog wel zijn :) ...

Een andere optie die de moeite waard is, is `res`. Deze bedient het resonantieniveau van de filter. Een hoge resonantie is een typisch kenmerk van de Acid Bass geluiden. Ons huidig niveau is `0.8`. Draai deze omhoog naar `0.85`, dan `0.9` en tenslotte naar `0.95`. Je zult hierdoor merken dat met een cutoff van `110` of hoger de verschillen makkelijker hoorbaar zullen zijn. Doe tenslotte maar eens goed gek, door bijvoorbeeld ´0.999´ te gebruiken, voor een krankzinnig geluid. Bij een `res` die zo hoog is zal de cutoff filter zo hard resoneren dat deze zelf gaat oscilleren en zo zelf een geluidsbron wordt!

Tenslotte veranderen we de golfvorm met de ´wave:´ opt naar `1`. Dit is de keuzeschakelaar van de oscillator. Standaard is deze `0`, deze is de zaagtand geluidsgolf (sawtooth wave).`1`is de pulsgolf (pulse wave) en `2`is de triangelgolf (triangle wave).

Je kan natuurlijk ook andere riffs gebruiken door de noten te veranderen in de ringen of zelfs noten te plukken uit toonladders en akkoorden. Veel plezier met je eerste Acid Bass synth.

## De TB-303 deconstrueren

Het ontwerp van de oorspronkelijke TB-303 is eigenlijk vrij eenvoudig. Zoals je in het volgende diagram ziet, zijn er maar 4 belangrijke kerndelen.

![TB-303 Design](../../../etc/doc/images/tutorial/articles/A.05-acid-bass/tb303-design.png)

Ten eerste is de wave oscillator - de grondstoffen van het geluid. In dit geval hebben we een blokgolf. Vervolgens is er de amplitude envelop van de oscillator, die de amplitude van de blokgolf door de tijd bedient. Deze zijn toegankelijk in Sonic Pi door de `attack:`, `decay:`, `sustain:` and `release:` opts vergezeld door het niveau hiervan. Lees voor meer informatie de sectie 2.4 'Duurtijd met enveloppen' in de ingebouwde handleiding. Vervolgens sturen we deze blokgolf met zijn envelop door een resonant low-pass filter. Dit dit snijdt in de hogere frequenties en heeft dat mooi resonantie-effect. Hier begint de pret. De grenswaarde van deze filter wordt ook gecontroleerd door eigen envelop! Dit betekent dat we een geweldige controle hebben over de klankkleur van het geluid door te spelen met beide van deze enveloppen. Laten we dit eens bekijken:

```
use_synth :tb303
with_fx :reverb, room: 1 do
  live_loop :space_scanner do
    play :e1, cutoff: 100, release: 7, attack: 1, cutoff_attack: 4, cutoff_release: 4
    sleep 8
  end
end
```
    
Voor elke standaard envelop opt, is er een 'cutoff_' gelijkwaardige opt-in de ': tb303' synth. Dus, om het tijdsverloop van de attack van de cutoff kunnen we de ' cutoff_attack:' opt gebruiken . Kopieer de code hierboven in een lege buffer en druk op Afspelen. Je hoort een gek geluidje in en uit kwelen. Begin hiermee te spelen te spelen. Probeer de ' cutoff_attack:' tijd naar te `1` te brengen en vervolgens naar `0.5`. Probeer daarna `8`.

Merk ook op dat ik alles door een `:reverb` FX heb gestuurd voor een extra sfeer, probeer zeker een ander FX uit en zie of dit werkt!

## Waardoor het allemaal samen valt

Tenslotte nog een stuk dat ik gecomponeerd heb met de ideeën van deze tutorial. kopieer deze in een lege buffer, luister er even naar en verander de parameters in een live coding. Kijk welke krankzinnige geluiden je ermee kan maken! Tot volgende keer...

```
use_synth :tb303
use_debug false
 
with_fx :reverb, room: 0.8 do
  live_loop :space_scanner do
    with_fx :slicer, phase: 0.25, amp: 1.5 do
      co = (line 70, 130, steps: 8).tick
      play :e1, cutoff: co, release: 7, attack: 1, cutoff_attack: 4, cutoff_release: 4
      sleep 8
    end
  end
 
  live_loop :squelch do
    use_random_seed 3000
    16.times do
      n = (ring :e1, :e2, :e3).tick
      play n, release: 0.125, cutoff: rrand(70, 130), res: 0.9, wave: 1, amp: 0.8
      sleep 0.125
    end
  end
end
```
