A.10 Control

# Jouw Geluid Controllen

Tot zover hebben we in deze serie de focus gelegd op het triggeren van geluiden. We hebben geleerd dat we de, in Sonic Pi, ingebouwde synth's met `play` of `synth` kunnen triggeren en vooraf opgenomen samples triggeren met `sample`. We hebben ook geleerd hoe we deze getriggerde klanken in een studio FX kunnen steken door middel van het `with_fx` commando. Combineer deze met het ongelofelijk nauwkeurige timing systeem van Sonic Pi en je kan een breed scala klanken,beats en riff's produceren. Maar zodra je een zorgvuldig geselecteerde klank opties hebt uitgekozen en deze hebt getriggerd, is er geen mogelijkheid om deze klank te veranderen terwijl deze speelt, juist? Neen hoor! Vandaag gaan we iets machtig leren, hoe een lopende synth controllen.

## Een Basis Klank

Laat ons even een leuke eenvoudige klank maken. Start Sonic Pi en typ in een nieuwe buffer het volgende:

```
synth :prophet, note: :e1, release: 8, cutoff: 100
```

Druk nu op de Afspeel-knop bovenaan links om dan een een mooie rommelend synth-geluid te mogen horen. Doe dit nog maar een paar keer om het in de vingers te krijgen en druk op Afspelen. Ok, klaar? Nu beginnen we het te controllen!

## Synth Nodes

Een weinig bekende functie in Sonic Pi is dat de fns `play`, `synth` en `sample`, een zogenaamde `SynthNode` opgeven, die een lopende klank vertegenwoordigd. Je kan zo'n `SynthNode` vastleggen door een standaard variabele te gebruiken en deze later te gaan **controllen**. We kunnen bijvoorbeeld de waarde van de `cutoff:` opt na 1 tel veranderen:

```
sn = synth :prophet, note: :e1, release: 8, cutoff: 100
sleep 1
control sn, cutoff: 130
```

Laten we de verschillende regels op zijn beurt even bekijken:

Ten eerste triggeren we de `:prophet` synth door de `synth` fn zoals gewoonlijk te gebruiken. We spelen het resultaat aan een variabele door genaamd `sn`. We zouden deze variabele ook een totaal andere naam hebben kunnen geven zoals `synth_node` of `kevin` , de naam speelt geen rol. Het is wel belangrijk dat deze voor jou een betekenis heeft als je een performance geeft en voor de mensen die jouw code lezen. Ik koos nu voor `sn` omdat dit een mooi geheugensteuntje is naar synth node.

Op regel 2 hebben we het standaard `sleep` commando. Het doet niks bijzonders, het vraagt de computer enkel om één tel te wachten alvorens verder te gaan naar de volgende regel.

Op regel 3 begint de pret met control. Hier gebruiken we de `control` fn, om onze lopende `synth_node` te vertellen, de cutoff waarde naar `130` te veranderen. Als je **Afspelen** klikt, hoor je de`:prophet` synth zoals daarvoor, maar na 1 tel zal deze verschuiven naar een meer heldere klank .

Moduleerbare Opties

De meeste van Sonic Pi's synths en FX opts kunnen veranderd worden nadat ze getriggerd zijn. Dit is echter niet voor allen het geval. Bijvoorbeeld, de envelop opts `attack:`, `decay:`, `sustain:` and `release:` kunnen alleen ingestelt worden bij bij het triggeren van de synth. Uitzoeken welke opts wel en welke niet kunnen veranderd worden is heel makkelijk, ga gewoon naar de documentatie voor de gegeven synth of FX en scroll naar beneden naar de afzonderlijke beschrijving van de opties.In de onderste regel vind je dan “Kan veranderd worden tijdens het afspelen” of “Kan niet veranderd worden eens ingesteld”. De documentatie voor de `:beep` synth maakt duidelijk dat het niet mogelijk is de `attack:` opt te veranderen eens ingesteld:

* Standaard instelling: 0
* Moet nul of groter dan nul zijn
* Kan niet worden aangepast eens ingesteld
* Geschaald met huidige BPM waarde

## Meerdere veranderingen

Als een synth eenmaal loopt ben je niet beperkt tot slechts 1 verandering - je kunt veranderingen blijven doen zoveel je wilt. bijvoorbeeld, we kunnen onze ':prophet' veranderen in een generator van een reeks accoordtonen (dat is een arpeggio, arpeggio generator =arpeggiator) door het volgende te doen:

```
notes = (scale :e3, :minor_pentatonic)
sn = synth :prophet, note: :e1, release: 8, cutoff: 100
sleep 1
16.times do
  control sn, note: notes.tick
  sleep 0.125
end
```

In dit stukje code kunnen we een aantal extra zaken toevoegen. Ten eerste hebben we een nieuwe variabele, genaamd `notes`, gedefinieerd, die de noten bevat die we graag willen doorlopen (een arpeggiator is gewoon een chique naam voor datgene waarin een lijst van noten in een volgorde wordt doorlopen) Ten tweede hebben we onze éénmalige oproep naar `control`, vervangen door een iteratie om deze 16 keer aan te roepen. In elke oproep naar `control`, hebben we een `:tick` -en we door onze ring van `notes`, die automatisch herhaald worden eens we aan het einde ervan gekomen zijn (dankzij de sublieme kracht van Sonic Pi's rings) Voor wat variatie probeer je `.tick` door `.choose` te vervangen, en bekijk of je hier een verschil kan horen.

Merk op dat we meerdere opts tegelijk kunnen veranderen. Probeer de control regel naar het volgende te wijzigen en hoor nu het verschil:

```
control sn, note: notes.tick, cutoff: rrand(70, 130)
```

## Sliden

Wanneer we een `SynthNode` controllen, reageert die precies op tijd vanuit de waarde van de opt naar de nieuwe waarde, alsof je een knop indrukte, of een schakelaar aanzette om de verandering aan te brengen. Dit kan ritmisch en percussief klinken, zeker als de opt een aspect van de klankkleur bepaalt zoals de `cutoff:`. Maar soms wil je helemaal niet dat de verandering in een knip gebeurd, maar dat deze langzaam aan overgaat van de huidige waarde naar de nieuwe waarde, alsof je langzaam een schuifregelaar bewoog. Natuurlijk kan Sonic Pi dit ook, door gebruik te maken van de `_slide:` opts.

Elke opt die gewijzigd kan worden heeft een overeenkomstige `_slide:` opt waarmee je de slide tijd mee kan instellen. Bijvoorbeeld, `amp:` heeft `amp_slide:` en `cutoff:` heeft `cutoff_slide:`. Deze slide opts werken een beetje anders dan de andere opts, doordat zij de synth noot opdraagt, hoe deze zich moeten gaan gedragen **de volgende keer dat ze gecontrold wordt**. Laten we hier even naar kijken:

```
sn = synth :prophet, note: :e1, release: 8, cutoff: 70, cutoff_slide: 2
sleep 1
control sn, cutoff: 130
```

Merk hierbij op dat we hetzelfde voorbeeld genomen hebben, maar hier hebben we `cutoff_slide:` toegevoegd. Dit betekent dat de volgende keer; de `cutoff:` opt wordt gecontrolt. Het neemt 2 beats (tellen) in beslag om te sliden vanuit de huidige waarde naar de nieuwe waarde. Daardoor hoor je, wanneer we `control` gebruiken, de cutoff sliden van 70 naar 130. Het creëert een interessante dynamiek in het geluid. Probeer nu de `cutoff_slide:` tiid te veranderen naar een kortere waarde zoals 0.5 en door een langere waarde zoals 4 om te zien hoe dit de klank verandert. Denk eraan, je kan elke modificeerbare opt op deze manier sliden, en elke `_slide:` waarde mag helemaal anders zijn. Zo kan de cutoff langzaam sliden, de amp snel sliden en de pan daar ergens tussenin sliden, zoals jij dit wil …

## Waardoor het allemaal samen valt

Laat ons naar een korter voorbeeld kijken dat ons de kracht toont van het controllen van synths nadat ze getriggerd zijn. Merk hierbij op dat ook FX zoals synths kan sliden maar met een iets andere syntax. Bekijk hoofdstuk 7.2 van de ingebouwde handleiding voor meer informatie omtrent het controllen van FX .

Kopieer de code naar een overgebleven buffer en luister hier naar. Stop niet en speel een beetje met de code. Verander de slide tijd, verander de noten, de synth, de rusttijd (sleep) en zorg dat je deze kan veranderen tot iets compleet anders!

```
live_loop :moon_rise do
  with_fx :echo, mix: 0, mix_slide: 8 do |fx|
    control fx, mix: 1
    notes = (scale :e3, :minor_pentatonic, num_octaves: 2).shuffle
    sn = synth :prophet , sustain: 8, note: :e1, cutoff: 70, cutoff_slide: 8
    control sn, cutoff: 130
    sleep 2
    32.times do
      control sn, note: notes.tick, pan: rrand(-1, 1)
      sleep 0.125
    end
  end
end
```
