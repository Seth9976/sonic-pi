A.11 Tik Tok

# De maat houden

Vorige maand hebben we in deze serie een diepe duik genomen in het randomiseer systeem dat Sonic Pi ondersteund. We gingen na hoe we dit kunnen gebruiken om op deterministische wijze, nieuwe lagen van dynamische controle op onze code, uit te oefenen.

## De beat tellen

Wanneer we muziek maken willen we soms dingen gaan doen die afhangen van het tempo en de beat. Sonic Pi heeft een speciaal systeem dat de beat telt, genaamd `tick`, om je de precieze controle te geven over wanneer een beat daadwerkelijk optreedt en zelfs meerdere beats met hun eigen tempo ondersteunt.

We spelen hier nu even mee, om de beats te laten toenemen hoeven we gewoon `tick` aan te roepen. Open een lege buffer, typ het volgende en druk op Afspelen:

```
puts tick #=> 0
```

Dit geeft je de huidige beat: `0`. Merk op dat zelfs wanneer je de Afspeel-knop herhaaldelijk indrukt, altijd `0` zal weergegeven worden. Dat komt omdat bij elke afspeel-commando het tellen van de beat van 0 start. We kunnen echter wanneer het afspelen nog actief is, de beat omhoog laten gaan, zoveel maal we zelf willen:

```
puts tick #=> 0
puts tick #=> 1
puts tick #=> 2
```

Telkens je het symbool `l#=>` aan het einde van een regel code ziet, betekend dit dat deze regel de tekst logt in het logvenster rechts. Bijvoorbeeld `puts eenderwat #=> 0 betekent dat de code `puts eenderwat `0`zal uitprinten naar het log op het moment dat dit in het programma voorkomt.

## De beat nagaan

We hebben gezien dat `tick` twee dingen doet. Het verhoogt (voegt toe) en geeft als resultaat de huidige beat. Soms willen we gewoon kijken naar de huidige beat zonder deze te verhogen. Dat kunnen we doen met `look`:

```
puts tick #=> 0
puts tick #=> 1
puts look #=> 1
puts look #=> 1
```

In deze code tick-en we de beat twee keer en roepen we `look` twee maal op. We zien de volgende waarden het log verschijnen: `0`, `1`, `1`, `1`. De eerste twee `tick`s gaven `0`en `1` zoals verwacht weer. daarna gaven de twee `look`s de laatste waarde van de beat weer, die `1`was.


## Ringen

Nu kunnen we dus de beat verhogen door middel van `tick`en deze nagaan door `look`. Maar wat nu? We hebben nog iets nodig om te laten tick-en. Sonic Pi gebruikt ringen om riffs, melodieën en ritmes te vertegenwoordigen, en het tick systeem is speciaal ontwikkeld om nauw met hen samen te werken. In feite hebben ringen hun eigen dot versie van `tick`,die twee dingen doet. Ten eerste functioneert deze als een gewone tick en verhoogt ook de beat.Ten tweede zoekt die de waarde van de ring op waarbij de beat als index wordt gebruikt. Laten we hier even naar kijken:

```
puts (ring :a, :b, :c).tick #=> :a
```

`.tick` 'is een speciale dot versie van 'tik', die de eerste waarde van de ring geeft `:a`. We kunnen elke waarden in de ring oppikken door `.tick` meerdere keren op te roepen:

```
puts (ring :a, :b, :c).tick #=> :a
puts (ring :a, :b, :c).tick #=> :b
puts (ring :a, :b, :c).tick #=> :c
puts (ring :a, :b, :c).tick #=> :a
puts look                   #=> 3
```

Neem nu een kijkje in het log en je zal `:a`, `:b`, `:c` zien en dan `:a` opnieuw. Merk op dat `look` `3` weergeeft. Een oproep naar `.tick` zal handelen als een gewone oproep naar `tick`, zij verhogen de beat hier.


## Een Live Loop Arpeggiator

De sterke kant hiervan komt naar boven als je `tick` mixt met `ring` en `live_loops`s. Wanneer zij gecombineerd worden, hebben we ons gereedschap om een eenvoudige arpegiator te bouwen,en om deze te begrijpen. We hebben maar vier dingen nodig:

1. Een ring die de te doorlopen noten bevat.
2. Een middel om de beat te verkrijgen en te verhogen.
3. De mogelijkheid om een noot te spelen volgens de huidige beat.
4. Een loop structuur waarin de arpegiator zich herhaalt.

Deze concepten vind je in de volgende code terug:

```
notes = (ring 57, 62, 55, 59, 64)
live_loop :arp do
  use_synth :dpulse
  play notes.tick, release: 0.2
  sleep 0.125
end
```

Laten we een kijkje nemen naar elke regels. Ten eerste definiëren we onze ring noten, die we continue laten afspelen. Dan maken we onze `live_loop` die `arp` heet, en die voor ons de loop doet. Elke cyclus van de `live_loop` stellen we onze synth in op `:dpulse´, om vervolgens de volgende noot in onze ring te spelen met behulp van `;tick`. Denk eraan dat dit onze beat-teller zal verhogen en onze laatste beat waarde zal gaan gebruiken als index in onze noten ring. Tenslotte, wachten we één achtste van een tel alvorens opnieuw te gaan loopen.

## Meerdere simultane beats

Belangrijk om weten is, is dat `tick` lokaal werkt tegenover `live_loop`. Dit betekent dat elke `live_loop` z'n eigen individuele beat-teller heeft. Dit is krachtiger dan het hebben van een globale metronoom en beat. Laten we dit in actie zien:

```
notes = (ring 57, 62, 55, 59, 64)
with_fx :reverb do
  live_loop :arp do
    use_synth :dpulse
    play notes.tick + 12, release: 0.1
    sleep 0.125
  end
end
live_loop :arp2 do
  use_synth :dsaw
  play notes.tick - 12, release: 0.2
  sleep 0.75
end
```

## Botsende Beats

Een grote oorzaak van verwarring met het tick systeem van Sonic Pi is wanneer men meerdere ringen willen tick-en in de zelfde `live_loop`:

```
use_bpm 300
use_synth :blade
live_loop :foo do
  play (ring :e1, :e2, :e3).tick
  play (scale :e3, :minor_pentatonic).tick
  sleep 1
end
```

Hoewel elke 'live_loop' zijn eigen onafhankelijke beat teller heeft, roepen we `.tick` twee keer op binnen dezelfde `live_loop`. Dit betekent dat de beat twee keer wordt verhoogd bij elke cyclus die doorlopen wordt. Op die manier kunnen interessante poly-ritmes ontstaan, maar dikwijls wil je dit ook helemaal niet. Je kan dit op twee manieren oplossen. De ene oplossing is een manuele oproep naar `tick` aan de start van de `live_loop` om dan `.look` te gebruiken om de huidige beat in elk van de `live_loop`'s op te zoeken. De tweede oplossing is elke `.tick` een unieke naam te geven zoals bvb `.tick(:naam1)`. Sonic Pi zal dan een aparte beat-teller maken en gebruiken voor elke, door jou benoemde `.tick`. Hiermee kan je zoveel verschillende beats werken als je maar wil! Zie sectie 9.4 over benoemde tick's in de ingebouwde handleiding voor meer informatie.

## Waardoor het allemaal samen valt

Laat ons de opgestoken kennis over `tick`s, `ring`s en `live_loop`s samenvoegen in een leuk voorbeeld. En ook deze keer mag je dit voorbeeld ook niet als een afgewerkt stuk zien. Verander hier en daar de waarden, en ontdek zo wat je er kan uitkrijgen. Tot volgende keer…

```
use_bpm 240
notes = (scale :e3, :minor_pentatonic).shuffle
live_loop :foo do
  use_synth :blade
  with_fx :reverb, reps: 8, room: 1 do
    tick
    co = (line 70, 130, steps: 32).tick(:cutoff)
    play (octs :e3, 3).look, cutoff: co, amp: 2
    play notes.look, amp: 4
    sleep 1
  end
end
live_loop :bar do
  tick
  sample :bd_ada if (spread 1, 4).look
  use_synth :tb303
  co = (line 70, 130, steps: 16).look
  r = (line 0.1, 0.5, steps: 64).mirror.look
  play notes.look, release: r, cutoff: co
  sleep 0.5
end
```
