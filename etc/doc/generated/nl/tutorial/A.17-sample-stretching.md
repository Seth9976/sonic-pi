A.12 Sample Stretching

# Sample Stretching

Wanneer mensen Sonic Pi ontdekken, een van de eerste dingen die ze leren, hoe eenvoudig het is om vooraf opgenomen geluiden af te spelen met behulp van de `sample` functie. Je kan bijvoorbeeld een industriële drumloop afspelen, het geluid van een koor horen of zelfs naar een kras op vinyl luisteren, allemaal via een enkele regel code. Veel mensen beseffen echter niet dat je eigenlijk de snelheid waarmee de sample wordt afgespeeld kan variëren wat voor een krachtige effect kan zorgen en de controle over je opgenomen geluiden naar een nieuw niveau kan tillen . Dus, start een kopie van Sonic Pi en laten ons aan de slag gaan met wat sample stretching!

## Samples vertragen

Om de afspeelsnelheid van de sample te wijzigen moeten we de `rate:` optie gebruiken:

```
sample :guit_em9, rate: 1
```
    
Als we een `rate:` waarde van `1` toekennen wordt de sample op de normale snelheid afgespeeld. Als we deze op halve snelheid willen afspelen gebruiken we simpelweg een `rate:` van`0.5`:


```
sample :guit_em9, rate: 0.5
```
    
Merk op dat dit twee gevolgen voor de audio heeft. Ten eerste zullen de samples lager in toonhoogte klinken en ten tweede duurt twee keer zo lang om ze af te spelen (Zie de zijbalk voor de uitleg waarom dit het geval is). We kunnen zelfs nog lager kiezen en lagere snelheden, naar '0' toe, een `rate:` van '0,25' is de kwart snelheid, '0.1' is voor een tiende van de snelheid, enz. Probeer af te spelen met enkele lage snelheden en zie of je het geluid naar een laag gerommel kan omzetten.

## Samples versnellen

Naast het maken van langere, lager klinkende samples door een kleiner waarde voor de snelheid te gebruiken, kunnen we hogere waarde ingeven om kortere en hoger klinkende geluiden te maken. Laten we dit nu even met een drumloop doen. Hoor even hoe dit klinkt met de standaard snelheid van `1`:

```
sample :loop_amen, rate: -1
```


Nu versnellen we deze een beetje:

```
sample :loop_amen, rate: 1.5
```
    
Ha! Nu verhuisden we naar een ander muzikaal genre, van old-skool techno naar jungle. Merk hierbij op dat zowel elke drumslag hoger klinkt als het gehele ritme versnelt. Probeer nu nog hogere waarden uit en zie hoe hoog en hoe kort je de drumloop kan maken. Als je bijvoorbeeld een snelheid van `100`gebruikt, verandert de drumloop naar een enkele klik!

## In achteruit versnelling

Ik weet zeker dat velen nu gaan denken... "wat als we nu eens een negatieve waarde gebruiken voor de `rate:`?" Goeie vraag! Als `1`de normale snelheid is en ´2´dubbele snelheid en waarbij `0,5` de halve snelheid is, dan moet `-1`achterwaarts zijn! Laten we dit met een snare uitproberen. Laten we deze eerst op normale snelheid afspelen:

```
sample :elec_filt_snare, rate: 1
```
    
Cool! Het speelt achterstevoren:

```
sample :elec_filt_snare, rate: -1
```
    
Natuurlijk kan je ook achterwaarts dubbel zo snel met een waarde van `-2` of achterwaarts op halve snelheid met `-0,5`. Speel nu even met verschillende negatieve waarden en amuseer je hiermee. Het is bijzonder grappig met de `:misc_burp` sample!


## Sample, Rate and Pitch [Sidebar]

Een van de effecten van het modificeren van de snelheid van samples, is dat bij hogere snelheden de klank hoger klinkt en bij trage snelheid lager. In het alledaagse leven hebt je misschien ook al eens opgemerkt dat wanneer een ziekenwagen voorbij rijd, als deze aankmt gereden zal de sirene hoger klinken dan wanneer deze voorbij is gereden- het zogenaamde Doppler effect. Waarom gebeurd dit?

Laten we even een simpele biep nemen die wordt vertegenwoordigd door een sinusgolf. Als we een oscilloscoop gebruiken om de grafische weergave van deze biep te kunnen zien krijgen we zoiets als Figuur B en bij een octaaf lager zien we zoiets als Figuur C. Merk op dat de golven van hogere noten compacter zijn en dat de golven van lagere noten meer uitgespreid zijn.

Een sample van een pieptoon is niets meer dan een heleboel cijfers (x, y, coördinaten) die wanneer ze uitgezet worden op een grafiek hun oorspronkelijke curven laten zien. Zie figuur D waarbij elke cirkel een coördinaat vertegenwoordigd. Om de coördinaten terug om te zetten in geluid, zet de computer elke x-waarde en de bijbehorende waarde van y naar de luidsprekers om. De truc hier is dat de afspeelsnelheid waarmee de computer zich door de x-nummers werkt niet dezelfde moet zijn als de snelheid waarmee zij werden geregistreerd. Met andere woorden, kan de ruimte (die de tijd vertegenwoordigen) tussen elke cirkel worden uitgerekt of samengedrukt. Dus, als de computer de x-waarden sneller dan het oorspronkelijke snelheid doorloopt, zal het het effect van het samendrukken van de cirkels in een hogere klinkende pieptoon resulteren. Het maakt ook de pieptoon korter maken gezien we sneller door de cirkels lopen. Dit wordt getoond in figuur E.

Tot slot, interessant om weten is dat, een wiskundige genaamd Fourier bewees dat elk geluid eigenlijk bestaat uit heel veel sinusgolven. Daarom, als we een opgenomen geluid comprimeren , rekken we vele sinusgolven allemaal op hetzelfde moment op precies deze manier uit.

## Pitch Bending

Zoals we hebben gezien, zal met een sneller tempo het geluid hoger in toonhoogte, en een trager tempo zal het geluid lager in toonhoogte zijn. Een zeer eenvoudige en nuttige truc is om te weten dat een verdubbeling van de rate waarde, eigenlijk resulteert in een toonhoogte van een octaaf hoger wordt en omgekeerd de halvering van de rate waarde resulteren in een toonhoogte van een octaaf lager. Dit betekent dat voor melodische samples, het naast elkaar afspelen op het dubbel/helft van de rate waarde, deze eigenlijk vrij mooi klinken:

```
sample :bass_trance_c, rate: 1
sample :bass_trance_c, rate: 2
sample :bass_trance_c, rate: 0.5
```
    
Maar, wat als we gewoon het tempo willen veranderen zodat de toonhoogte stijgt met een halve toon (één noot hoger op een piano)? Sonic Pi maakt dit zeer eenvoudig via de ' rpitch:' optie:

```
sample :bass_trance_c
sample :bass_trance_c, rpitch: 3
sample :bass_trance_c, rpitch: 7
```
    
Als je een kijkje neemt op het log aan de rechterkant, zal je merken dat een ' rpitch:' van '3' overeenkomt met een snelheid van '1.1892' en een ' rpitch:' van '7' overeenkomt met een snelheid van '1.4983'. Tot slot kunnen we zelfs de ' rate:' en ' rpitch:' optie combineren:

```
sample :ambi_choir, rate: 0.25, rpitch: 3
sleep 3
sample :ambi_choir, rate: 0.25, rpitch: 5
sleep 2
sample :ambi_choir, rate: 0.25, rpitch: 6
sleep 1
sample :ambi_choir, rate: 0.25, rpitch: 1
```
    

## Waardoor het allemaal samen valt

Laat ons even kijken naar een eenvoudig stukje waarbij deze ideeën gecombineerd worden . Kopieer deze naar een lege Sonic Pi buffer en klik op afspelen, luister even en gebruik deze dan als uitgangspunt voor je eigen stuk muziek. Merk hoe leuk het is om de snelheid van het afspelen van samples te manipuleren. Als extra oefening probeer je je eigen geluiden op te nemen en speel met de rate waarden om te zien wat voor gekke geluiden je hiermee kan maken.

```
live_loop :beats do
  sample :guit_em9, rate: [0.25, 0.5, -1].choose, amp: 2
  sample :loop_garzul, rate: [0.5, 1].choose
  sleep 8
end
 
live_loop :melody do
  oct = [-1, 1, 2].choose * 12
  with_fx :reverb, amp: 2 do
    16.times do
      n = (scale 0, :minor_pentatonic).choose
      sample :bass_voxy_hit_c, rpitch: n + 4 + oct
      sleep 0.125
    end
  end
end
```







