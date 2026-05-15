A.6 Muzikale Minecraft

# Muzikale Minecraft


**Niet meer beschikbaar** * Excuses, maar dit artikel werd teruggeschreven toen Minecraft Pi Edition nog steeds deel uitmaakte van Raspberry Pi OS. Sonic Pi had toen ingebouwde ondersteuning om het met code te bedienen. Helaas is dit niet langer het geval en zijn de minecraft specifieke code, (de `mc_*` functies) niet meer beschikbaar in Sonic Pi. Dit artikel wordt hier echter achtergelaten als herinnering aan wat ooit mogelijk was.*


Hallo en welkom terug! In de vorige tutorials heb we gericht puur op de mogelijkheden rond muziek in Sonic Pi - ( Je Raspberry Pi in een muziekinstrument veranderen). Tot nu toe hebben we geleerd geleerd hoe te:

* Live Code - ter plekke wijzigen van het geluid,
* Geweldige Beats coderen,
* Krachtige synth leads, genereren
* De bekende TB-303 recreëren.

Er is zoveel meer om te laten zien (die we in toekomstige versies zullen onderzoeken). Echter, deze maand, gaan we eens kijken wat je met Sonic Pi kunt doen en je waarschijnlijk niet weet: controle hebben over Minecraft.

## Hello Minecraft World

We gaan aan de slag. Start je Raspberry Pi, alsook Minecraft Pi en creëer een nieuwe wereld. Start Sonic Pi nu op en schik de vensters van deze programma's, zodat je zowel Sonic Pi als Minecraft Pi gelijkertijd kan zien.

In een nieuwe buffer typ je dan het volgende:

```
mc_message "Hello Minecraft from Sonic Pi!"
```
    
Klik nu op afspelen. En Bam! Jouw bericht verschijnt nu in Minecraft! Was dat niet makkelijk? Speel hier nu mee met je eigen berichten. Veel plezier ermee!

![Screen 0](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-0-small.png)

## Sonic Teleporter

Nu gaan we op verkenning. De standaard optie is werken met de muis en het klavier en beginnen rondlopen. Dat werkt wel, maar het is behoorlijk traag en saai. Het zou veel beter zijn moesten we een soort teleportatie machine hebben. Wel, dankzij Sonic Pi, hebben we dat ook. Probeer deze:

```
mc_teleport 80, 40, 100
```
    
Jeetje! Dat duurde even. Als je niet in de vliegende-modus zou zijn, was je die afstand helemaal naar beneden gevallen. Als je space twee keer aanslaat om in vliegmodus te gaan en opnieuw te teleporteren, blijf je zweven op de plaats waar je naartoe zapte.

Nu, wat betekenen deze cijfers ? We hebben drie getallen die de coördinaten beschrijven, waar in de wereld we willen naartoe gaan . We geven elk nummer een naam - x-, y- en z:

* x - hoe ver links en rechts (80 in ons voorbeeld)
* y - hoe hoog we willen zijn (40 in ons voorbeeld)
* z - hoe ver voorwaarts of achterwaarts (100 in ons voorbeeld)

Door verschillende waarden te kiezen voor x, y en z kunnen we "overal" in de onze wereld teleporteren. Probeer maar! Kies hierbij verschillende getallen en kijk waar je terechtkomt. Wanneer je scherm zwart wordt betekent dit dat je jezelf onder de grond of in een berg hebt geteleporteerd. Kies dan gewoon een een hoger getal voor y om terug op land te komen. Ga door tot je op een leuke plek terecht gekomen bent...

Met behulp van de tot nu toe aangereikte ideeën, gaan we nu een Sonic Teleporter die een leuk teleportatie geluid maakt als door de Minecraft wereld zoeven:

```
mc_message "Preparing to teleport...."
sample :ambi_lunar_land, rate: -1
sleep 1
mc_message "3"
sleep 1
mc_message "2"
sleep 1
mc_message "1"
sleep 1
mc_teleport 90, 20, 10
mc_message "Whoooosh!"
```
    
![Screen 1](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-1-small.png)

## Magische blokken

Nu je een leuke plek gevonden hebt, gaan we beginnen bouwen. Je kan nu zoals je gewoonlijk doet verwoed gaan klikken om blokken onder de cursor te plaatsen. Of je kan de magie van Sonic Pi gebruiken. Probeer dit:

```
x, y, z = mc_location
mc_set_block :melon, x, y + 5, z
```

Kijk omhoog! Er hangt een meloen in de lucht! Neem even de tijd om naar de code te kijken. Wat hebben we hier gedaan? Op lijn één namen we de huidige locatie van Steve als de variabelen x, y en z. Deze komen overeen met onze coördinaten hierboven beschreven. Wij gebruik deze coördinaten in de fn 'mc_set_block', die de blok van uw keuze zal plaatsen op de opgegeven coördinaten.Als we deze nu omhoog willen plaatsen, moeten we de waarde van y verhogen,dat is ook de reden waarom we er 5 aan hebben toegevoegd. Laten we hiervan een lang spoor maken:

```
live_loop :melon_trail do
  x, y, z = mc_location
  mc_set_block :melon, x, y-1, z
  sleep 0.125
end
```

Nu, spring nu naar Minecraft, zorg ervoor dat je in de vliegende-modus bent (2 keer spatie indien niet) en vlieg over de hele wereld. Kijk achter je om een mooi parcours van meloen blokken te zien! Kijk wat voor gedraaide patronen je in de lucht kan maken.

## Minecraft Live Coderen

Wie in de laatste maanden de zelfstudie (tutorials) heeft gevolgd zal nu wel behoorlijk onder de indruk zijn. Het spoor van meloenen is dan ook cool, maar het strafste deel van het voorgaande voorbeeld is dat je `live_loop` kan gebruiken met Minecraft! Als je `live_loop` niet kent, het is Sonic Pi's magische eigenschap die geen enkele andere programmeertaal heeft. Het staat je toe verschillende "loops" tezelfdertijd af te spelen, en deze tezelfdertijd ook aan te passen terwijl zij afspelen. Ze zijn heel krachtig en leuk. Ik gebruik `live_loop` om live muziek te spelen in nachtclubs met Sonic Pi. Dj's gebruiken disc's en ik gebruik `live_loop`s :-) Maar vandaag gaan we zowel muziek als Minecraft live coderen.

Laten we beginnen. Voer de bovenstaande code in en begin weer met het maken van uw meloen parcours . Nu, zonderde code te stoppen , wijzigen `:melon` naar `:brick en klik afspelen. En hupsaké, je maakt nu een parcours van baksteen. Hoe eenvoudig was dat! Zetten we er wat muziek bij? Ook gemakkelijk. Probeer dit:

```
live_loop :bass_trail do
  tick
  x, y, z = mc_location
  b = (ring :melon, :brick, :glass).look
  mc_set_block b, x, y -1, z
  note = (ring :e1, :e2, :e3).look
  use_synth :tb303
  play note, release: 0.1, cutoff: 70
  sleep 0.125
end
```
    
Nu, verander de code terwijl dit afspeelt. Verander de blok type's - probeer `:water`, `:grass` of jouw favoriete blok type uit. Probeer ook de waarde van de cutoff te wijzigen van `70`naar `80`en nog hoger tot `100`. Leuk niet?

## Waardoor het allemaal samen valt

![Screen 2](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-2-small.png)

Laten ons nu alles wat we tot hiertoe hebben gezien combineren, met nog een extra vleugje magie. En we combineren onze teleportatie met het blok plaatsen en met de muziek om tot een Minecraft Muziek Videoclip te komen. Wees niet bezorgd dat je niet alles begrijpt, typ het in en speel ermee door sommige waarden te veranderen terwijl het live afspeelt. Veel plezier en tot een volgende keer...
    
```
live_loop :note_blocks do
  mc_message "This is Sonic Minecraft"
  with_fx :reverb do
    with_fx :echo, phase: 0.125, reps: 32 do
      tick
      x = (range 30, 90, step: 0.1).look
      y = 20
      z = -10
      mc_teleport x, y, z
      ns = (scale :e3, :minor_pentatonic)
      n = ns.shuffle.choose
      bs = (knit :glass, 3, :sand, 1)
      b = bs.look
      synth :beep, note: n, release: 0.1
      mc_set_block b, x+20, n-60+y, z+10
      mc_set_block b, x+20, n-60+y, z-10
      sleep 0.25
    end
  end
end
live_loop :beats do
  sample :bd_haus, cutoff: 100
  sleep 0.5
end
```
