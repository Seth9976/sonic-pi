A.1 Trucuri pentru Sonic Pi

# Cele mai bune 5 trucuri

## 1. Nu exista greseli

Cea mai importanta lectie pe care trebuie sa o inveti despre Sonic Pi este ca nu exista greseli. Cea mai buna metoda de a invata este sa incerci iar si iar. Incerca diferite variante, nu te concentra pe cum arata codul si experimenteaza cu cat mai multe sintetizatoare, note, efecte si parametri pentru acestea. Vei descoperi o gramada de combinatii care te vor face sa razi pentru ca suna caraghios dar si adevarate nestemate care suna incredibil. Arunca la cos ce nu-ti place si pastreaza ce e reusit. Cu cat indraznesti sa faci mai multe 'greseli' cu atat mai repede vei invata si-ti vei descoperi stilul personal.


## 2 Foloseste efectele

Stapanesti deja bazele Sonic Pi pentru a crea sunete cu 'sample' si 'play'? Ce urmeaza? Stiai ca Sonic Pi ofera suport pentru mai mult de 27 de efecte de studio pentru a schimba felul in care suna programul tau? Efectele sunt ca niste filtre de imagine fanteziste, doar ca in loc sa incetosezi imaginea sau sa o faci alb/negru poti adauga sunetului reverberatii, distorsiuni sau ecou. E ca si cum ai conecta iesirea de la chitara la pedala de efecte preferata si apoi la amplificator. Din fericire, Sonic Pi face aplicarea efectelor si mai simpla, fara cabluri! Tot ce trebuie sa faci este sa decizi la ce bucata de cod vrei sa aplici efectul si sa o incadrezi cu codul speficic pentru acesta. Sa vedem un exemplu. Sa spunem ca ai urmatorul cod:

```
sample :loop_garzul
16.times do
  sample :bd_haus
  sleep 0.5
end
```

Daca vrei sa adaugi un efect la esantionul  `:loop_garzul`, trebuie doar sa-l introduci in interiorul unui bloc 'with_fx' ca mai jos:

```
with_fx :flanger do
  sample :loop_garzul
end
16.times do
  sample :bd_haus
  sleep 0.5
end
```

Acum, daca vrei sa adaugi un efect la toba bas, introdu-o si pe aceasta intr-un 'with_fx':

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

Retine, poti impacheta *orice* cod intr-un bloc 'with_fx' si toate sunetele create vor fi transformate de acel efect.


## 3. Modifica parametrii sintetizatoarelor

Pentru a-ti descoperi stilul personal vei dori desigur sa afli cum sa modifici si sa controlezi sintetizatoarele si efectele. De exemplu, ai putea dori sa modifici durata unei note, sa adaugi mai mult reverb sau sa schimbi duratele dintre ecouri. Din fericire, Sonic Pi iti ofera un nivel de control incredibil pentru a realiza exact aceste lucruri folosind parametrii optionali (opts). Sa aruncam o privire rapida. Copiaza acest cod intr-un buffer si apasa Run:

```
sample :guit_em9
```

O, un minunat sunet de chitara. Acum, sa ne jucam cu el. Ce-ar fi sa-i schimbam viteza de redare?

```
sample :guit_em9, rate: 0.5
```

Hei, ce e chestia aia de la sfarsit - 'rate: 0.5'? Acesta este un parametru optional. Toate sintetizatoarele si efectele suporta astfel de parametri si exista o multime din care poti alege. Iata cum ii poti folosi pentru un efect:

```
with_fx :flanger, feedback: 0.6 do
  sample :guit_em9
end
```

Acum incearca sa modifici in 1 valoarea pentru feedback, ca sa auzi niste sunete traznite! Citeste documentatia pentru mai multe detalii despre multimea de parametri disponibili.


## 5. Programeaza live

Cea mai buna metoda de a exersa este sa programezi live. Asta iti permite sa incepi de la o bucata oarecare de cod si sa o modifici si sa o reglezi in timp ce ruleaza. De exemple, daca nu stii ce face parametrul cutoff pentru un esantion, joaca-te cu el. Sa incercam! Copiaza acest cod intrun buffer din Sonic Pi:

```
live_loop :experiment do
  sample :loop_amen, cutoff: 70
  sleep 1.75
end
```

Acum apasa pe Run si vei auzi o bataie de toba infundata. Acum, schimba valoarea pentru 'cutoff:' la '80' si apasa din nou pe Run. Auzi diferenta? Incearca `90`, `100`, `110`...

Odata ce ai prins miscarea cu 'live_loop' nu vei mai renunta la el. De cate ori improvizez live ma bazez pe 'live_loop' la fel cum un tobosar se bazeaza pe betele lui. Pentru mai multe informatii despre programarea live vezi sectiunea 9 din tutorial.

## 5. Lasa-te purtat de curenti la intamplare

Un alt lucru pe care imi place sa-l fac este sa trisez putin lasand Sonic Pi sa compuna pentru mine. Un mod foarte interesant de a face asta este sa folosesti randomizarea. Poate suna complicat, dar chiar nu este. Sa aruncam o privire. Copiaza asta intr-un buffer:

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

Acum, cand vei reda asta, vei auzi un sir constant de note din gama `:e2 :minor_pentatonic`redate de sintetizatorul `:dsaw`. "Stai, stai! Asta nu este o melodie", parca te aud strigand! Ei bine, asta este prima parte a unui truc magic. De fiecare data cand reluam bucla 'live_loop' putem cere Sonic Pi sa readuca sirul aleator la un punct prestabilit. E ca si cum te-ai intoarce in timp folosind TARDIS - masina timpului din Doctor Who - la un anumit punct in timp si spatiu. Sa incercam: adauga linia `use_random_seed 1` la `live_loop`:

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

Acum, de fiecare data cand se reia bucla 'live_loop', sirul aleator este resetat. Asta inseamna ca va alege aceleasi 16 note de fiecare data. A fost rapid! Am obtinut instantaneu o melodie. Acum urmeaza partea si mai incitanta. Schimba valoarea pentru seed din '1' in alt numar. Sa zicem '4923'. Uau! Alta melodie! Deci schimband un singur numar (cel care initializeaza secventa aleatoare) poti explora atatea combinatii melodice cate iti poti imagina! Aceasta este magia programarii.
