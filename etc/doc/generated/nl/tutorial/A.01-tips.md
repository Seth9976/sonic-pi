A.1 Tips voor Sonic Pi

# Vijf Top Tips

## 1. Er bestaan geen fouten

De belangrijkste les om leren in Sonic Pi is dat er geen echte fouten bestaan. De beste manier van leren is om gewoon te proberen en te proberen en te proberen. Probeer verschillende dingen uit, stop ermee je zorgen te maken of je code nu goed klinkt of niet en begin te experimenteren met zoveel mogelijk verschillende synths, noten, FX als mogelijk is. Je zal ontdekken dat je een heleboel dingen maakt die je aan het lachen brengen omdat het gewoon verschrikkelijk klinkt en soms zullen er echte juweeltjes tussen zitten, die echt geweldig klinken. Laat gewoon de dingen vallen die u niet graag doet en hou van wat je doet. Hoe meer `fouten` je jezelf toelaat te maken hoe sneller je zaken zal leren en jouw persoonlijke geluids codering zal ontdekken.


## 2. FX gebruiken

Laat ons aan nemen dat je de basis om klanken te maken in Sonic Pi met `sample`, `play`. Wat nu? Wist je dat Sonic Pi over de 27 studio ondersteunt om de sound van je code te veranderen. Effecten zijn zoals beeldfilters in tekenprogramma's maar in plaats van zaken te vervagen of zwart-wit te maken, kan je zaken aan jouw geluid toevoegen zoals reverb, distortion, en echo. Het concept isq zoals je een geluidskabel vanuit je gitaar in de effectenpedaal van jou keuze te steken en vervolgens de versterker in. Gelukkig maakt Sonic Pi je het heel makkelijk om FX te gebruiken en dit zonder kabels! Het enige wat je moet doen is het fragment in jouw code selecteren waar je FX wil op gebruiken en deze met de FX code omhullen. Laten we naar een voorbeeld kijken. Stel dat je deze code hebt:

```
sample :loop_garzul
16.times do
  sample :bd_haus
  sleep 0.5
end
```

Als je een FX aan de `:loop_garzul` sample wil toevoegen, moet je deze gewoon in een `with_fx` blok steken zoals hier:

```
with_fx :flanger do
  sample :loop_garzul
end
16.times do
  sample :bd_haus
  sleep 0.5
end
```

Als je nu ook een FX aan de bass drum wil voegen, omhul deze dan ook met een `with_fx`:

```
with_fx :flanger do
  sample :loop_garzul
end
with_fx :echo do
  16.times do
    sample :bd_haus
    sleep 0.5
  end
end
```

Denk eraan , je kan *elke* code omhullen met `with_fx` en elke klank hierin gecreëerd zal door deze FX passeren.


## 3. Het parametreren van jouw synths

Om je eigen geluid te gaan ontwikkelen, zal je snel willen leren, hoe je synths en Fx kan veranderen en controllen. Zo wil je bijvoorbeeld de lengte van een noot veranderen, of wil je wat meer reverb toevoegen, of de tijdsinstellingen van een echo aanpassen. Gelukkig geeft Sonic Pi de mogelijkheid om dit op een hoog niveau te gaan doen met behulp van optionele parameters of in het kort: opts. Laten we dit even bekijken. Kopieer de volgende code in een werkveld en klik afspelen:

```
sample :guit_em9
```

Ooh, een mooie gitaarklank! Nu gaan we hiermee spelen. Wat dacht je van zijn koers te veranderen?

```
sample :guit_em9, rate: 0.5
```

Hé, wat is dat stukje `rate: 0.5`, dat daar op het einde staat? Dat is een opt! Alle synths en FX van Sonic Pi ondersteunen deze en er zijn er tal van beschikbaar om mee te spelen. Probeer deze eens:

```
with_fx :flanger, feedback: 0.6 do
  sample :guit_em9
end
```

Probeer nu de feedback te verhogen naar 1 om wat gekke geluiden te horen. Lees de documentatie voor details over de beschikbare opties.


## 4. Live Code

De beste manier om zaken snel te ontdekken in Sonic Pi is experimenteren en live te coderen. Dit laat jou toe om stukjes code op te starten en deze voortduren te veranderen en te controllen terwijl je speelt. Bijvoorbeeld, als je niet weet wat de cutoff parameter doet met een sample, speel er gewoon mee, probeer het. Kopieer deze code in één van je werkvelden:

```
live_loop :experiment do
  sample :loop_amen, cutoff: 70
  sleep 1.75
end
```

Klik nu op afspelen en hoor hierbij een licht gedempte drum break. Verander de cutoff waarde nu naar `80` en klik opnieuw op afspelen. Hoor je het verschil? Probeer `90`, `100`, `110`...

Eens je het gewoon bent `live_loop`s te gebruiken zal je nooit meer zonder deze kunnen. Wanneer ik een live coding doe reken ik hier zwaar op, zoals een drummer zijn drumsticks nodig heeft. Voor meer informatie over live coding, kijk naar Sectie 9 van de ingebouwde handleiding.

## 5. Surfen op random golven

Tot slot nog deze, ik hou ervan om een beetje vals te spelen door Sonic Pi in mijn plaats te laten componeren. Een goede manier om dit te verwezenlijken is gebruik te maken van het randomiseren. Misschien lijkt dit een beetje ingewikkeld maar dat is het helemaal niet. Laat ons hiernaar naar kijken. Kopieer deze in een lege buffer:

```
live_loop :rand_surfer do
  use_synth :dsaw
  notes = (scale :e2, :minor_pentatonic, num_octaves: 2)
  16.times do
    play notes.choose, release: 0.1, cutoff: rrand(70, 120)
    sleep 0.125
  end
end
```

Wanneer je deze afspeelt zal je een constante stroom van random noten horen spelen in de toonladder `:e2 :minor_pentatonisch` gespeeld met de synth `:dsaw`." Maar wacht! Dat is geen melodie" hoor ik jou roepen! Wel, dit is ook het eerste deel van onze tovertruc. Elke keer we rond gaan in de `live_loop`, kunnen we Sonic Pi vertellen om de random toeloop naar een gegeven punt te resetten. Dit is een beetje zoals in de tijd teruggaan met de dokter in de TARDIS naar een bepaald punt in tijd en ruimte. We proberen de lijn `use_random_seed 1` toe te voegen aan de `live_loop`:

```
live_loop :rand_surfer do
  use_random_seed 1
  use_synth :dsaw
  notes = (scale :e2, :minor_pentatonic, num_octaves: 2)
  16.times do
    play notes.choose, release: 0.1, cutoff: rrand(70, 120)
    sleep 0.125
  end
end
```

Nu, elke keer de `live_loop` rond gaat , wordt de random stream gereset. Dit betekend dat iedere keer dezelfde 16 noten gekozen worden. En presto! Instant melodie. Nu komt het spannende deel. Verander de seed waarde van `1` naar een ander getal. Neem `4923`. Wauw! Een andere melodie! Dus door gewoon één cijfer te veranderen (de random seed), je kan je zoveel melodische combinaties gaan verkennen als je je kan voorstellen ! Nu, dat is de magie van code.
