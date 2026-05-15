4 Randomiseren

# Randomiseren

Een geweldige manier om jouw muziek interessant te maken is het gebruik van willekeurige getallen. Sonic Pi heeft een goede functionaliteit om willekeurigheid in jouw muziek toe te voegen, maar eerst moeten we naakte waarheid onder ogen zien: in Sonic Pi is *random geen echte random*. Wat heeft dit nu weer te betekenen? Laat ons eens kijken.

## Herhaalbaarheid

Een heel nuttige willekeurige functie is 'rrand', welke je een willekeurige waarde tussen twee nummers geeft - een *min* en *max*. ('rrand' is kort voor ranged random (willekeurigen binnen een bepaald bereik)). Laten we proberen een willekeurige opmerking te spelen:

```
play rrand(50, 95)
```

Ooh het speelt een willekeurige noot. Het speelde de noot `83.7527`. Een mooie willekeurige noot tussen 50 en 95. Hu, heb ik nu net de willekeurige noot die jij ook had exact voorspeld? Hier is iets verdacht gaande. Probeer de code nu nog eens opnieuw uit te voeren. Wat?? Het koos `83.7527` opnieuw? Niet echt random dus!

Het antwoord is, dat deze niet echt random is, maar pseudo-random. Sonic Pi zal jou random-achtige cijfers geven op een repetitieve manier. Dit is heel nuttig om ervoor te zorgen dat jouw muziek,gemaakt op jou toestel, hetzelfde klinkt op iemand anders toestel, ook al heb je de functie van randomiseren gebruikt in je compositie.

Natuurlijk, bij een zeker stukje muziek, moest het elke keer 'random' `83.7527` kiezen, dan zou het niet interessant meer zijn. Maar dat doet het niet. Probeer het volgende:

```
loop do
  play rrand(50, 95)
  sleep 0.5
end 
```

Ja, Eindelijk klinkt het random. Binnen een gegeven doorloop gaan de volgende opvragingen naar random functies randum waarden weergeven. Bij de daar opvolgende doorloop gaan zij echter exact dezelfde sequentie van deze random waarden spelen en dus net hetzelfde klinken. Alsof Sonic Pi terug in de tijd ging naar exact hetselfde punt elke keer de afspeelknop werd ingedrukt. De "Groundhog Day" in de wereld van muziek synthese!

## "Haunted Bells"

Een mooie illustratie van randomiseren in actie is het voorbeeld "haunted bell's" die de `:perc_bell`' sample laat "loop-en" met een willekeurige rate en sleep tijd tussen de klokkengeluiden:

```
loop do
  sample :perc_bell, rate: rrand(0.125, 1.5)
  sleep rrand(0.2, 2)
end
```

## Random cutoff

Een ander leuk voorbeeld van randomiseren is het willekeurig wijzigen van de cutoff van een synth. Een geweldige synth om dit op uit te proberen is de ': tb303' emulator:

```
use_synth :tb303
loop do
  play 50, release: 0.1, cutoff: rrand(60, 120)
  sleep 0.125
end
```

## Random "seeds"

Dus, wat als deze bijzondere reeks willekeurige getallen die Sonic Pi biedt je niet bevalt ? Nou is het volledig mogelijk om een ander uitgangspunt te kiezen via 'use_random_seed'. De standaard seed instelling is 0, kies dus een andere seed voor een andere ervaring in randomisering!

Houd rekening met het volgende:

```
5.times do
  play rrand(50, 100)
  sleep 0.5
end
```

Telkens wanneer u deze code uitvoert, zult u dezelfde sequentie van 5 noten horen. Voor een andere volgorde verander gewoon de seed:

```
use_random_seed 40
5.times do
  play rrand(50, 100)
  sleep 0.5
end
```

Dit zal een andere sequentie van 5 noten produceren. Door het veranderen van de seed en door het beluisteren van de resultaten vindt u wel iets dat u goed vind - en wanneer u deze met anderen deelt, horen zij precies wat u ook hebt gehoord.

Laten we eens kijken naar enkele andere nuttige randomisering functies.


## choose

Een zeer gebruikelijk actie zou zijn, random te kiezen uit een lijst van gekende items. Bijvoorbeeld, ik zou één van de volgende noten willen spelen: 60, 65 of 72. Ik kan dit bewerkstelligen met `choose` die mij een item uit een lijst laat kiezen. Eerst moet ik mijn getallen in een lijst zetten, hetgeen ik kan doen door deze in rechthoekige haakjes te plaatsen en door middel van komma's te scheiden: : `[60, 65, 72]. Vervolgens moet ik deze doorgeven aan `choose`:

```
choose([60, 65, 72])
```

Laten we eens horen hoe dat klinkt:

```
loop do
  play choose([60, 65, 72])
  sleep 1
end
```

## rrand

We hebben `rrand` al gezien, maar laat ons deze nog even overlopen. Deze reproduceert een willekeurig getal tussen twee getallen die hierbij exclusief zijn. Dit wil zeggen dat deze getallen,noch het onderste, noch het bovenrste zal weergeven worden, maar altijd een getal tussen die twee. Het getal zal altijd een niet-geheel getal zijn- wat betekend dat dit nooit een geheel getal zal zijn, maar een breuk van een getal. Voorbeelden van niet-gehele getallen die worden gereproduceerd door `rrand(20, 110)`:

* 87.5054931640625
* 86.05255126953125
* 61.77825927734375

## rrand_i

Soms zal je een geheel getal, willen doen uitkomen in plaats van een niet-geheel getal in de ranomisering. Hier komt `rrand_i` redding brengen. Dit werk het zelfde als `rrand` behalve dat deze hier de min en max waarden als potentiële random waarden laat uitkomen (inclusief ipv. exclusief bereik). Voorbeelden van getallen die gereproduceerd kunnen worden door `rrand` zijn:

* 88
* 86
* 62

## rand

Dit geeft een willekeurig niet-geheel getal tussen 0 (inbegrepen) en de max waarde die u opgeeft (exclusieve). Standaard wordt een waarde tussen 0 en 1 geretourneerd. Daarom is deze nuttig voor het kiezen van willekeurige ' amp:' waarden:

```
loop do
  play 60, amp: rand
  sleep 0.25
end
```

## rand_i

Vergelijkbaar met de relatie tussen 'rrand_i' en 'rrand', 'rand_i' geeft een willekeurig geheel getal tussen 0 en de max waarde die u opgeeft.

## dice (teerling)

Soms wil je een gooi van een dobbelstenen nabootsen - dit is een speciaal geval van 'rrand_i' waar de laagste waarde altijd 1 is . Een oproep naar de 'dobbelstenen' moet je het aantal zijden op de dobbelsteen opgeven. Een standaard dobbelsteen heeft 6 zijden, en zal 'dice(6)' zich ook zo gaan gedragen - en waarden van ofwel 1, 2, 3, 4, 5 of 6 reproduceren.Maar misschien wil je wel, net zoals in een fantasy rollenspel , een 4-zijdige dobbelstenen, of een 12 zijdige dobbelstenen, of een 20 zijdige dobbelsteen - misschien zelfs een 120 zijdige dobbelsteen!

## one_in

Tot slot wil je ook de gooi naar het hoogste getal nabootsen, zoals onze 6 op een normale dobbelsteen. `one_in` zal daarom de waarde true met een kans van één op het aantal zijden op de dobbelsteen reproduceren. Daarom geeft `one_in(6)` true met een waarschijnlijkheid van 1 in 6 terug, en anders false . True en False waarden zijn zeer nuttig voor `if` verklaringen die wij in een latere sectie van dit leerprogramma zullen behandelen.

Maak maar even een wirwar aan Random-heid!
