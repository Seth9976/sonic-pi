A.3 Beat-uri programate

# Beat-uri programate

Una dintre cele mai incitante si mai revolutionare inventii in muzica moderna a fost sampler-ul. Acesta este o cutie care iti permite sa inregistrezi orice sunet pe car apoi il poti manipula si reda in diferite moduri interesante. De exemplu, poti sa iei o inregistrare veche, sa gasesti un solo de toba (break), sa o memorezi in sampler si apoi sa o redai in bucla la viteza injumatatita ca sa ai baza pentru cel mai nou beat al tau. Asa s-a nascut muzica hip-hop iar astazi este aproape imposibil sa gasesti muzica electronica in care nu exista macar un tip de esantion (sample). Folosirea esantioanelor este o metoda foarte buna de a introduce elemente noi si interesante in interpretarea bazata pe programare live.

De unde putem lua un sampler? Ai deja unul - este Raspberry Pi! Aplicatia Sonic Pi pentru programare live care vine preinstalata are inclus in nucleu un sampler foarte puternic. Sa ne jucam cu el!

## Amen Break

Unul dintre solourile de toba cele mai usor de recunoscut se numeste Amen Break. A fost interpretat prima data in 1969 in piesa "Amen Brother" de Winstons ca parte a soloului de toba. Dar abia cand a fost descoperit de muzicienii de la inceputurile hip-hop-ului in anii 80 si a fost fost folosit in samplere a inceput sa fie folosit din greu in multe alte stiluri, cum ar fi drum & bass, breakbeat, hardcore techno sau breakcore.

Sunt sigur ca te bucuri sa afli ca este inclus si in Sonic Pi. Goleste un buffer si pune acolo urmatorul cod:

```
sample :loop_amen
```

Apasa *Run* si boom! Asculti unul dintre solourile de toba cu cea mai mare influenta din istoria muzicii dance. Totusi, acest esantion nu a devenit faimos pentru redarea normala, el a fost construit pentru redarea in bucla.


## Intinderea unui beat

Sa redam in bucla Amen Break folosind vechea noastra cunostinta 'live_loop' prezentata in acest tutorial:

```
live_loop :amen_break do
  sample :loop_amen
  sleep 2
end
```

Ok, acum se repeta, dar exista o pauza deranjanta de fiecare data. Asta se intampla deoarece am cerut o pauza de '2' batai iar la ritmul implicit de 60 BPM esantionul ':loop_amen' dureaza '1.753' batai. Avem deci liniste timp de '2 - 1.753 = 0.247' batai. Desi dureaza putin, este sesizabila.

Pentru a rezolva aceasta problema putem folosi parametrul 'beat_strecth: pentru a cere Sonic Pi sa intinda (sau sa comprime) esantionul astfel incat sa aiba numarul specificat de batai.

Functiile 'sample' si 'synth' din Sonic Pi iti ofera un control puternic prin intermediul parametrilor optionali ca `amp:`, `cutoff:` si `release:`. Totusi, numele de parametru optional este destul de lung si il putem prescurta folosind *opt* pentru simplitate.

```
live_loop :amen_break do
  sample :loop_amen, beat_stretch: 2
  sleep 2
end  
```

Acum dansam! S-ar putea totusi sa dorim sa marim sau sa scadem viteza pentru a se potrivi cu atmosfera.

## Sa ne jucam cu timpul

Ce facem daca vrem sa schimbam stilul in hip hop clasic sau breakcore? O metoda simpla este sa ne jucam cu timpul - cu alte cuvinte sa umblam la tempo. E foarte usor sa faci asta in Sonic Pi - doar adaugi o comanda 'use_bpm' in bucla live:

```
live_loop :amen_break do
  use_bpm 30
  sample :loop_amen, beat_stretch: 2
  sleep 2
end 
```

Cat timp asculti ritmul asta lent, ai timp sa remarci ca pauza este tot de 2, dar acum avem 30 BPM si totusi totul este sincronizat. Parametrul 'beat_stretch' tine cont de noul ritm pentru a se asigura ca totul functioneaza.

Acum urmeaza partea cu adevarat interesanta. In timp ce bucla este inca live, schimba '30' din linia 'use_bpm 30' in '50'. Uau, totul a devenit mai rapdi dar *a ramas sincronizat*. Incearca ceva mai rapid - urca la 80, la 120, fa o nebunie si pune 200!


## Filtrarea

Acum putem reda esantioane in bucle live, sa vedem care sunt cei mai interesanti parametri pentru 'sample'. Primul este 'cutoff:' care controleaza filtrul de cutoff al sampler-ului (care taie frecventele mai inalte decat limita stabilita). Implicit este dezactiva, dar il poti porni cu usurinta:

```
live_loop :amen_break do
  use_bpm 50
  sample :loop_amen, beat_stretch: 2, cutoff: 70
  sleep 2
end  
```

Modifica parametrul 'cutoff'. De exemplu, mareste-l la 100, apasa *Run* si asteapta ca bucla sa se reia ca sa auzi care este efectul modificarii. Vei observa ca valori mici, ca 50, suna infundat si mai spre bas, iar valorile gen 100 sau 120 au un sunet mai complet si mai clar. Asta deoarece parametrul 'cutoff:' va duce la tairea partilor de frecventa inalta din sunet, asa cum masina de tuns taie varful firelor de iarba. Parametrul 'cutoff:' este asemanator cu lungimea la care este setata masina de tuns - spune cat sa ramana din firele de iarba.


## Slicing

O alta unealta cu care ne putem juca este efectul slicer. Acest taie (slice) sunetul in bucatele. Incadreaza linia 'sample' cu codul pentru efect ca mai jos:

```
live_loop :amen_break do
  use_bpm 50
  with_fx :slicer, phase: 0.25, wave: 0, mix: 1 do
    sample :loop_amen, beat_stretch: 2, cutoff: 100
  end
  sleep 2
end
```

Observi ca sunetul variaza in sus si in jos ceva mai mult. (Poti asculta sunetul original, fara efect, schimband valoarea pentru 'mix:' cu '0'). Acum, incearca sa te joci cu parametrul 'phase:'. Acesta reprezinta frecventa (in batai) cu care se aplica efectul slicing. O valoare mai mica, gen '0.125', va taia sunetul mai rapid, iar valori mai mari, gen '0.5' vor face taierea mai lenta. Observi ca injumatatind sau dubland de mai multe ori valoarea pentru 'phase:' sunetul va suna mereu bine. La final, schimba valoarea 'wave:' in 0, 1 sau 2 pentru a auzi cum schimba sunetul. Acestea sunt diferite forme pentru semnal. 0 inseamna forma de fierastrau (crestere brusca, scadere lenta), 1 inseamna patrat (crestere brusca, scadere brusca), iar 2 inseamna triunghi (crester lenta, scadere lenta).


## Sa punem totul cap la cap

Acum sa facem o calatorie in timp si sa ne intoarcem in Bristol la inceputurile curentului drum and bass cu exemplul de luna aceasta. Nu-ti bate capul cu ce inseamna asta, doar introdu codul, apasa Run si incepe sa-l modifici live jucandu-te cu valorile pentru parametri ca sa vezi ce iese. Te rog sa arati si altora ce ai reusit sa creezi. Pe data viitoare...

```
use_bpm 100
live_loop :amen_break do
  p = [0.125, 0.25, 0.5].choose
  with_fx :slicer, phase: p, wave: 0, mix: rrand(0.7, 1) do
    r = [1, 1, 1, -1].choose
    sample :loop_amen, beat_stretch: 2, rate: r, amp: 2
  end
  sleep 2
end
live_loop :bass_drum do
  sample :bd_haus, cutoff: 70, amp: 1.5
  sleep 0.5
end
live_loop :landing do
  bass_line = (knit :e1, 3, [:c1, :c2].choose, 1)
  with_fx :slicer, phase: [0.25, 0.5].choose, invert_wave: 1, wave: 0 do
    s = synth :square, note: bass_line.tick, sustain: 4, cutoff: 60
    control s, cutoff_slide: 4, cutoff: 120
  end
  sleep 4
end
```
