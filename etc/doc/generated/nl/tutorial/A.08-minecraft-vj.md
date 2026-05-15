A.8 Word een Minecraft VJ

# Word een Minecraft VJ

![Screen 0](../../../etc/doc/images/tutorial/articles/A.08-minecraft-vj/minecraft-vj-0-small.png)


**Niet meer beschikbaar** * Excuses, maar dit artikel werd teruggeschreven toen Minecraft Pi Edition nog steeds deel uitmaakte van Raspberry Pi OS. Sonic Pi had toen ingebouwde ondersteuning om het met code te bedienen. Helaas is dit niet langer het geval en zijn de minecraft specifieke code, (de `mc_*` functies) niet meer beschikbaar in Sonic Pi. Dit artikel wordt hier echter achtergelaten als herinnering aan wat ooit mogelijk was.*


Iedereen heeft ooit Minecraft gespeeld. Je zal allemaal verbazingwekkende structuren gebouwd hebben, sluwe vallen ontworpen hebben en zelfs cart lines hebben uitgewerkt met hun bijbehorende redstone schakelaars. Hoeveel van jullie hebben al een optreden gedaan met Minecraft? Wedden dat je niet wist dat je Minecraft kunt gebruiken om ongelofelijke beelden af te spelen net zoals een professionele VJ dat doet.

Als de muis de enige manier was om Minecraft te modificeren, had je het waarschijnlijk moeilijk om de dingen vlug genoeg veranderd te krijgen. Gelukkig komt jouw Raspberry Pi met een versie van Minecraft dat met behulp van code kan gecontrolt worden. Er is ook een app bij die Sonic Pi heet, die Het coderen van Minecraft niet alleen makkelijk, maar ook plezierig maakt.

In het artikel van vandaag gaan we jou enkele van de tips en trucjes tonen, die we hebben gebruikt bij optredens in nachtclubs en podia over de hele wereld.

Laten we starten...

## Starten

Laten we eerst een opwarmertje doen, en even terug keren naar de basis. Vooraleerst, doe je Raspberry Pi open en start zowel Minecraft als Sonic Pi op, creëer een nieuwe wereld, en kies in Sonic Pi een verse buffer en schrijf deze code:

```
mc_message "Let's get started..."
```
    
Sla de afspelen toets aan om het bericht te zien in het Minecraft-venster. Ok, we zijn klaar om te starten. We gaan wat pret maken...

## Zandstormen

Wanneer we Minecraft gebruiken om visuals mee te maken, houden we in gedachte dat we iets leuk om naar te kijken maken als zowel makkelijk uit code kunnen genereren. Een leuke truc is een zandstorm creëren door zandblokken uit de lucht te laten vallen. Daarvoor hebben we een aantal basis fns nodig:

* `sleep` - voor het invoegen van een vertraging tussen acties
* `mc_location` - om onze huidige lokatie te vinden
* `mc_set_block`- om de zandblokken op een bepaalde plaats te zetten
* `rrand` - om ons toe te laten random waarden binnen een bepaald bereik te genereren
* `live_loop` - om ons toe te laten een constante stroom aan regen te maken

Als je niet vertrouwd bent met een van de ingebouwde fns zoals `rrand`, typ het woord in je buffer, klik erin en gebruik de combinatie `control-i` op je toetsenbord om de ingebouwde documentatie op te roepen. Alternatief kan je ook navigeren naar de *lang* tab in het hulpsysteem en zoek fns op, tussen al de andere leuke dingen die je kan doen.

Laat het een beetje regenen vooraleer je de zandstorm op volle kracht zet. Pluk je huidige locatie en gebruik deze om in de buurt een paar zandblokken in de lucht te plaatsen:

```
x, y, z = mc_location
mc_set_block :sand, x, y + 20, z + 5
sleep 2
mc_set_block :sand, x, y + 20, z + 6
sleep 2
mc_set_block :sand, x, y + 20, z + 7
sleep 2
mc_set_block :sand, x, y + 20, z + 8
```
    
Wanneer je Afspelen aanslaat, moet je misschien een beetje rondkijken om zicht te hebben op de vallende blokken vermits hun zichtbaarheid afhangt van jouw kijkrichting. Geen nood als je hun val net gemist hebt, klik gewoon terug op Afspelen voor een nieuwe partij zandregen- zorg er gewoon voor dat je in de juiste richting kijkt!

Laten we even kijken wat hier gaan de is. In de eerste regel bemachtigde we de coördinaten van de huidige locatie van Steve via de `mc_location` en plaatsten we deze in de variabelen `x`, `y`, en `z`. Op de volgende regels gebruikten we `mc_set_block` fn om wat zand te plaatsen op de zelfde coördinaten als die van Steve, met wat aanpassingen. We kozen dezelfde x coördinaat , een y coördinaat 20 blokken hoger en achtereenvolgens een groter z coördinaat zodat het zand niet ver van Steve naar beneden valt.

Waarom nemen we deze code niet over om hier wat mee te gaan spelen? Probeer meer lijnregels, verander de rusttijden (sleep), probeer `:sand` met `:gravel` te mengen en gebruik verschillende coördinaten? Experimenteer maar en veel plezier ermee!

## Live Loops ontketend

Ok, tijd om een storm te laten woeden door de kracht van de `live_loop` te laten ontketenen - Sonic Pi's wonderlijke eigenschap die de kracht van het live code-en laat zien- namelijk de code wijzigen terwijl deze loopt!

```
live_loop :sand_storm do
  x, y, z = mc_location
  xd = rrand(-10, 10)
  zd = rrand(-10, 10)
  co = rrand(70, 130)
  synth :cnoise, attack: 0, release: 0.125, cutoff: co
  mc_set_block :sand, x + xd, y+20, z+zd
  sleep 0.125
end
```
    
Hoe leuk! We gaan behoorlijk snel door deze Loop (8 keer per seconde) en bij elke doorloop vinden we opnieuw de locatie van Steve om er dan 3 random waarden bij te genereren:

* `xd` - het verschil bij x welke tussen -10 en 10 liggen
* `zd` - het verschil bij z ook tussen -10 en 10
* `co` - een cutoff waarde voor de low pass filter tussen 70 en 130

We gebruiken dan deze random waarden in de fns `synth` en `mc_set_block` die ons vallend zand geven op willekeurige plaatsen rond Steve samen met een regen-achtig geluid van de `:cnoise` synth.

Voor diegene voor wie live loops nieuw zijn - hier begint de pret pas met Sonic Pi. Terwijl de code loopt en het zand naar beneden wordt uitgestrooid, tracht je één van de waarden te veranderen, misschien de rusttijd (sleep) naar `0.25` of de `:sand` block type naar `:gravel. Druk nu *opnieuw* Afspelen in. En presto veranderde de zaken die je had aangepast zonder de code te hoeven stoppen. Dit is jouw toegangspoort om als een echte VJ aan de slag te gaan. Blijf oefenen en verander hier en daar wat. Hoe verschillend kan jij de visuals maken zonder de code te stoppen?

## Epische blok patronen

![Screen 1](../../../etc/doc/images/tutorial/articles/A.08-minecraft-vj/minecraft-vj-1-small.png)

Wat tenslotte ook een fantastische manier is om interessante visuals te maken is van een grote muur van een patroon te genereren om naartoe en langsheen te vliegen. Om dit effect te bereiken moeten moeten we ons van het idee van het willekeurig plaatsen van blokken verplaatsen naar deze geordend te plaatsen. We kunnen dit doen door twee sets van iteratie (druk op de hulp-knop om naar sectie 5.2 van de zelfstudie "Iteratie en loops" te navigeren voor meer achtergrondinformatie over iteraties). De gekke `|xd|` achter do betekend dat `xd` achter elke waarde van de iteratie zal worden gezet. De eerste keer zal deze 0 zijn, dan1, dan 2.....enz. Door twee delen iteratie te gaan nesten, kunnen we op deze manier de coördinaten voor een vierkant genereren. We kunnen dan willekeurig blok-types kiezen van een ring van blokken om een interessant effect te bekomen:

```
x, y, z = mc_location
bs = (ring :gold, :diamond, :glass)
10.times do |xd|
  10.times do |yd|
    mc_set_block bs.choose, x + xd, y + yd, z
  end
end
```

Behoorlijk gaaf. Terwijl we hiermee plezier beleven, probeer je `bs.choose` te veranderen naar `bs.tick om van een willekeurig patroon een meer regelmatig patroon te bekomen. Probeer ook de blok-type's te veranderen en de waaghalzen onder ons willen deze misschien ook in een `live_loop` plakken om de patronen automatisch te laten veranderen.

Nu, voor de VJ finale: verander de twee `10.times` naar `100.times` en klik op Afspelen. Kaboom! Een reusachtig en gigantische muur van willekeurig geplaatste bakstenen. Beeld je in hoe lang je daarmee bezig zou zijn geweest indien je dit met de muis zou gedaan hebben! Sla twee keer de spacebar om naar vlieg-modus over te gaan en begin aan een duikvlucht voor geweldige visuele effecten. Maar stop op dit punt nog niet, gebruik je verbeeldingsvermogen voor coole ideeën en gebruik in Sonic Pi de kracht van het coderen om deze werkelijk te maken. Wanneer je voldoende geoefend hebt, dim je de lichten en zet een VJ show voor je vrienden op!
