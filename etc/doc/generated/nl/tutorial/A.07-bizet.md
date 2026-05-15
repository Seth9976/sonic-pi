A.7 Bizet Beats

# Bizet Beats

Na onze korte excursie vorige maand, naar de fantastische wereld van het coderen van Minecraft met Sonic Pi , gaan we opnieuw de muzikale toer op. Vandaag gaan we een klassieke opera dansstuk rechtstreeks naar de 21ste eeuw katapulteren met behulp van de ongelofelijke krachtt van code.

## Schandalig en Disruptief

Laten we door de teletijdmachine terugkeren naar het jaar 1875. Een componist Bizet genaamd heeft net zijn laatste opera Carmen afgewerkt. Zoals het helaas dikwijls gaat met vernieuwende muziek ,sloeg het bij het publiek niet aan wegens te verschillend en te uitbundig. Helaas stierf Bizet 10 jaar voordat deze opera een internationaal succes werd en één van de meest beroemde en gespeelde opera's aller tijden werd. Uit sympathie voor deze tragedie gaan we één van de hoofdthema's van Carmen in een moderne muziekstijl gieten die voor de meeste mensen in deze tijd ook te "anders" is: live gecodeerde muziek!

## De Habanera decoderen

Om deze volledige opera zou in deze oefening een te grote uitdaging zijn, laten we daarom ons op één van de meest beroemde delen focussen- de baslijn van de Habanera:

![Habanera Riff](../../../etc/doc/images/tutorial/articles/A.07-bizet/habanera.png)

Dit kan uiterst onleesbaar lijken als je geen muziek hebt gestudeerd .Als programmeurs kunnen wij muzieknotatie zien als gewoon een andere vorm van code.Hiermee geeft u instructies aan een musicus in plaats van een computer. Daarom moeten wij een manier bedenken om het te decoderen.

## Noten

De noten zijn van links naar rechts gerangschikt zoals de woorden op uw scherm,maar hebben ook verschillende plaatsing in hoogte.* De hoogte in de partituur geeft de toonhoogte weer.* Hoe hoger de noot in de partituur staat, hoe hoger de noot klinkt.

Hoe we in Sonic Pi de toonhoogte van noten moeten veranderen, weten we al - ofwel gebruiken we getallen zoals `play 75` en `play 80` of we gebruiken de naam van de noot: `play :E` en`play :F`. Gelukkig vertegenwoordigt elk van de verticale posities van de partituur de naam van een specifieke noot. Neem een kijkje naar deze handige opzoek tabel:

![Notes](../../../etc/doc/images/tutorial/articles/A.07-bizet/notes.png)

## Rusttijden

Partituren zijn een zeer rijke en expressieve soort code waarmee je verschillende uiteenlopende zaken kan communiceren.Het zal dan ook niet verbazen dat partituren jou niet alleen vertellen welke noten je moet spelen, maar ook waar je *geen* noten mag spelen. In het programmeren is dat te vertalen naar het idee van `nil` of `null` - de afwezigheid van een waarde. Met andere woorden, een noot niet spelen is zoals een noot die afwezig is.

Als je de partituur aandachtig bekijkt zal je zien dat deze een combinatie van zwarte punten met lijnen bevat de te spelen noten vertegenwoordigen, en kronkelende dingen die de rusttijden vertegenwoordigen. Gelukkig heeft Sonic Pi een handige manier om een rusttijd aan te geven: `:r`, dus als we `play :r` afspelen, speelt deze eigenlijk stilte af! We kunnen ook `play :rest` schrijven, `play nil` of `play false` die allemaal evengoed een rusttijd vertegenwoordigen.

## Ritme

Tenslotte nog een laatste manier om de noten te ontcijferen: de timing van de noten. In de originele partituur zal je zien dat de noten verbonden zijn met dikke lijnen die men waardestrepen noemt. De tweede noot heeft twee lijnen in deze waardestreep wat betekent dat deze noot een zestiende van een tel aanhoudt.De andere noten hebben een enkel waardestreepje, wat wil zeggen dat deze een 8-ste van een tel aanhouden. De rest heeft twee krullige lijnen, vlagjes, die willen zeggen dat deze ook een 16-de van een tel zijn.

Als we trachten nieuwe zaken te ontcijferen, is het handig om alles eenvormig te maken om zo patronen en verbanden te kunnen zien. Bijvoorbeeld, als we onze notenschrift in louter zestiende noten zouden herschrijven, kan je zien dat ons notenschrift verandert in een opeenvolging van noten en rusttijden.

![Habanera Riff 2](../../../etc/doc/images/tutorial/articles/A.07-bizet/habanera2.png)

## De Habanera Hercoderen

We staan nu op het punt om te beginnen met het vertalen van deze baslijn naar Sonic Pi. Laten we deze noten en rusttijden in een ring coderen:

```
(ring :d, :r, :r, :a, :f5, :r, :a, :r)
```
    
Laten we eens kijken hoe dit klinkt. Gooi het in een live loop en "tick" erdoor heen:

```
live_loop :habanera do
  play (ring :d, :r, :r, :a, :f5, :r, :a, :r).tick
  sleep 0.25
end
```
    
Fantastisch, die herkenbare riff komt door jouw luidsprekers meteen tot leven. Het kostte veel moeite om hier te komen, maar het was het waard - high five!
    
## Moody Synths

Nu we de baslijn hebben, gaan we de sfeer van de opera recreëren. Een synth om uit te proberen is `:blade` welke een stemmige, jaren 80 stijl, synth lead is. We proberen deze met de beginnoot `:d`en laten deze door een slicer en een reverb passeren:

```
live_loop :habanera do
  use_synth :fm
  use_transpose -12
  play (ring :d, :r, :r, :a, :f5, :r, :a, :r).tick
  sleep 0.25
end
with_fx :reverb do
  live_loop :space_light do
    with_fx :slicer, phase: 0.25 do
      synth :blade, note: :d, release: 8, cutoff: 100, amp: 2
    end
    sleep 8
  end
end
```

Probeer nu de andere noten in de baslijn: `:a`en `:f5`. Denk eraan, je hoeft niet op stop te drukken, je hoeft enkel de code te veranderen terwijl de muziek afspeelt en op afspelen de drukken. Probeer ook verschillende waarden uit voor de `phase:` opt van de slicer uit zoals `0.5`, `0.75`en `1`.

## Waardoor het allemaal samen valt

Ten slotte combineren we alle ideeën die we tot dusver hebben gezien in een nieuwe remix van De Habanera. Je zal opmerken dat ik een ander deel van de baslijn in een commentaarlijn heb gestoken. Eens je alles in een nieuwe buffer hebt getypt, druk je op afspelen om de compositie te horen. Nu, zonder op stop te drukken ga je de tweede regel *uitcommentariëren* door de # te verwijderen en opnieuw op afspelen te drukken, - hoe prachtig is dat!. Probeer zelf nog het een en ander en veel plezier ermee.

```
use_debug false
bizet_bass = (ring :d, :r, :r, :a, :f5, :r, :a, :r)
#bizet_bass = (ring :d, :r, :r, :Bb, :g5, :r, :Bb, :r)
 
with_fx :reverb, room: 1, mix: 0.3 do
  live_loop :bizet do
    with_fx :slicer, phase: 0.125 do
      synth :blade, note: :d4, release: 8,
        cutoff: 100, amp: 1.5
    end
    16.times do
      tick
      play bizet_bass.look, release: 0.1
      play bizet_bass.look - 12, release: 0.3
      sleep 0.125
    end
  end
end
 
live_loop :ind do
  sample :loop_industrial, beat_stretch: 1,
    cutoff: 100, rate: 1
  sleep 1
end
 
live_loop :drums do
  sample :bd_haus, cutoff: 110
  synth :beep, note: 49, attack: 0,
    release: 0.1
  sleep 0.5
end
```
