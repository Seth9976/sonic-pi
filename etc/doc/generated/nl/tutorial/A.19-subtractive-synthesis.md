A.19 Sound Design - Subtractieve Synthese

# Subtractieve synthese

Dit is het tweede in een serie artikelen over het gebruik van Sonic Pi in sound design. Vorige maand hebben we gekeken naar additieve synthese waar we met eenvoudige handelingen, met het tegelijkertijd afspelen van meerdere geluiden, kunnen zorgen voor een nieuw gecombineerde geluid. We kunnen bijvoorbeeld verschillend klinkende synths of zelfs dezelfde synth op verschillende toonhoogten gebruiken om een nieuw complex geluid op te bouwen met eenvoudige ingrediënten. Deze maand nemen we een kijkje naar een nieuwe techniek die in de volksmond *subtractieve synthese* wordt genoemd, wat gewoon het nemen van een bestaand complex geluid is en deze ontdoen van bepaalde onderdelen, om er iets nieuws van te maken . Dit is een techniek die meestal wordt geassocieerd met het geluid van analoge synthesizers uit de jaren 1960 en 1970, maar ook bekend is door de recente opleving van modulaire analoge synths via populaire nieuwe standaarden zoals bijvoorbeeld Eurorack.

Ondanks dat dit klinkt als een bijzonder ingewikkelde en geavanceerde techniek, maakt Sonic Pi het je verrassend simpel en eenvoudig, we beginnen er gelijk mee.

## Complex Bron Signaal

Om een geluid goed met subtractieve synthese te laten werken, moet deze meestal vrij rijk en interessant op zich klinken. Dat betekent niet dat deze ongelofelijk complex hoeft te zijn, in feite zullen een typische `:square` of`:saw`golf volstaan:

```
synth :saw, note: :e2, release: 4
```

Merk hier op dat dit geluid al vrij interessant is en veel verschillende frequenties boven `:e2` (de tweede E op een piano) bevat, die aan toevoeging doen aan het maken van de klankkleur. Als dat nog niet veel duidelijk maakt, probeer dit te vergelijken met de `:beep`toon:

```
synth :beep, note: :e2, release: 4
```

Vermits de `:beep` synth een eenvoudige sinusgolf is, zal je een meer zuivere toon horen op `:e2` en geen van de meer knapperige/zoemende geluiden die je met `:saw`hoorde. Het is dit bezige, variërende geluid van de sinus golf, die we kunnen gaan gebruiken voor subtractieve synthese.

## Filters

Eens we ons eerste rauwe geluid hebben gevonden, is onze volgende stap deze door een filter te sturen, die delen van het geluid wegneemt of vermindert. Een van de meest gebruikte filters bij subtractieve synthese is de zogenaamde low pass filter. Deze laat alle lage tonen van het geluid door, maar zal de hogere frequenties reduceren of weglaten. Sonic Pi heeft een krachtig maar makkelijk te gebruiken FX systeem waar een low pass filter inzit, namelijk de `:lpf`. Laten we hiermee even spelen:

```
with_fx :lpf, cutoff: 100 do
  synth :saw, note: :e2, release: 4
end
```

Als je goed luistert zal je horen dat wat geroezemoes en knapperigheid verwijderd is. In feite zijn alle frequenties in het geluid boven `100`gereduceerd of weggenomen en zijn enkel de frequenties daaronder aanwezig in het geluid. Probeer een `cutoff:` te vinden om noten te verlagen met bijvoorbeeld `70` en dan naar `50` en vergelijk dan deze klank.

Uiteraard is de `:lpf` niet de enige filter die je kan gebruiken om een inkomend signaal te bewerken. Een andere belangrijke FX is de high pass filter aangeduid als `:hpf` in Sonic Pi. Dit doet het tegenovergestelde van`:lpf` waar die de hoge delen van het geluid doorlaat en/of afsnijdt, doet deze dat voor de lage delen.

```
with_fx :hpf, cutoff: 90 do
  synth :saw, note: :e2, release: 4
end
```

Merk op dat het geluid nu drukker en rasperig is nu alle lage frequenties weggenomen zijn. Speel maar even met de cutoff waarden en merk op dat lagere waarden meer van de originele basstonen doorlaten en bij hogere waarden, het geluid dunner en stiller wordt.

## Low Pass Filter

![Varying amounts of low pass filtering](../../../etc/doc/images/tutorial/articles/A.19-subtractive-synthesis/subtractive-synthesis-waveforms.png)

Het low-pass filter is zo'n belangrijk onderdeel in elke gereedschapskist van de subtractieve synthese, dat het moeite waard is iets dieper in te gaan op hoe het werkt. Dit diagram toont dezelfde geluidsgolf (de ': prophet ' synth) met verscheidene filter hoeveelheden . Aan de top toont deel A de audiogolf zonder filtering. Merk op hoe de golfvorm erg puntig is en veel scherpe randen bevat. Het zijn deze harde, scherpe hoeken die de hoge scherpe delen van het geluid produceren. Sectie B toont het low-pass filter in actie - merk hoe deze minder puntige en meer afgerond heeft, dan de golfvorm hierboven. Dit betekent dat het geluid minder hoge frequenties heeft, waardoor het een meer zacht afgerond aanvoelt . Sectie C toont de low-pass filter met een vrij lage grenswaarde - dit betekent dat zelfs nog meer van de hoge frequenties zijn verwijderd uit het signaal wat resulteert in een nog zachtere, ronder golfvorm. Tot slot merk je hoe de vorm van de golf, die de amplitude vertegenwoordigt, afneemt als we van A naar C gaan. Subtractieve synthese werkt door het verwijderen van delen van het signaal en betekent dus dat de totale amplitude wordt verminderd door de waarde van de filter die verhoogt.


## Filter Modulatie

Tot nu toe klonk ons geluid vrij statisch. Met andere woorden, het geluid verandert op geen enkele manier gedurende zijn gehele duurtijd. Vaak zal je wat beweging in je geluid willen, om wat leven in het timbre te brengen. Een van de manieren om dit te bewerkstelligen is met filtermodulatie, d.w.z. het veranderen van de filter opties gedurende een bepaalde tijd. Sonic Pi heeft gelukkig een krachtig instrument om de FX opt's, tijdsgebonden te manipuleren. Je kan bijvoorbeeld voor elke moduleerbare opt een bepaalde tijd opgeven, hoe traag of hoe snel deze van een bepaalde waarde, naar de vooropgestelde waarde verschuift:

```
with_fx :lpf, cutoff: 50 do |fx|
  control fx, cutoff_slide: 3, cutoff: 130
  synth :prophet, note: :e2, sustain: 3.5
end
```

Laat ons even kijken naar wat hier gebeurt. Eerst starten we een `:lpf` FX blok met een initiële `cutoff:` zeer laag op`20`. De eerste regel heeft evenwel een mysterieuze `|fx|` op het einde. Dit is een optioneel onderdeel van de `with_fx` syntax wat ons in staat stelt om een naam te geven aan de lopende FX synth, en deze te manipuleren. Dat is precies wat regel 2 doet, namelijk de `cutoff_slide:` opt naar 4 zetten, en de uiteindelijke `cutoff:` naar `130` te brengen. Het FX zal de `cutoff:` opt's waarde van `50` naar `130` schuiven, over een periode van 3 tellen. Ten slotte triggeren we een synth, die als bron door deze low pass filter wordt gestuurd.


## Waardoor het allemaal samen valt

Dit is slechts een zeer basic voorproefje van wat mogelijk is als je filters gebruikt om een geluidsbron te wijzigen. Probeer in Sonic Pi met de vele ingebouwde FX te spelen, om te zien wat een gekke geluiden je hiermee kan ontwerpen. Als je geluid te statisch aanvoelt , vergeet niet dat je de opties kan moduleren om bewegingen aan te brengen.

Laten we eindigen, met het ontwerpen van een functie die een nieuw geluid maakt met subtractieve synthese. Zie of je kan achterhalen wat hier aan de hand is - en voor de gevorderde Sonic Pi lezers: - kijk of je kunt vinden waarom ik alles in de oproep verpak naar `at` (stuur de antwoorden naar @samaaron op Twitter).

```
define :subt_synth do |note, sus|
  at do
    with_fx :lpf, cutoff: 40, amp: 2 do |fx|
      control fx, cutoff_slide: 6, cutoff: 100
      synth :prophet, note: note, sustain: sus
    end
    with_fx :hpf, cutoff_slide: 0.01 do |fx|
      synth :dsaw, note: note + 12, sustain: sus
      (sus * 8).times do
        control fx, cutoff: rrand(70, 110)
        sleep 0.125
      end
    end
  end
end
subt_synth :e1, 8
sleep 8
subt_synth :e1 - 4, 8
```
