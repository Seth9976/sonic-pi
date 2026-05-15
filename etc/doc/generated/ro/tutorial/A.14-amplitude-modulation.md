A.14 Modulatia in amplitudine

# Modulatia in amplitudine

Luna asta vom analiza in detaliu  unul dintre cele mai puternice efecte audio ale Sonic Pi - ':slicer'. La sfarsitul acestui articol vei sti sa modifici volumul global al diferitelor parti din sunetul programat live in moduri noi foarte puternice. Asta iti va permita se creezi noi structuri ritmice si timbrale si sa iti largesti capacitatea de exprimare.

## Modifica amplitudinea cu ':slicer'

Ce face de fapt efectul ':slicer'? Poti sa iti imaginezi ca cineva se joaca cu volumul la televizorul tau sau la amplificatorul audio. Sa aruncam o privire, dar mai intai sa ascultam murmurul produs de bucata asta de cod care foloseste sintetizatorul ':prophet':

```
synth :prophet, note: :e1, release: 8, cutoff: 70
synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
```

Acum, sa il trecem printr-un efect ':slicer':

```

with_fx :slicer do
  synth :prophet, note: :e1, release: 8, cutoff: 70
  synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
end
```

Observa cum efectul slicer actioneaza ca si cand ar opri/porni sunetul intr-un ritm regulat. De asemenea, vei vedea ca ':slicer' afecteaza toate sunetele generate intre blocurile 'do'/'end'. Poti controla viteza cu care porneste si opreste sunetul cu parametrul 'phase:' care este o prescurtare de la durata fazei. Valoarea implicita este '0.25' ceea ce inseamna de 4 ori pe secunda la BPM implicit de 60. Sa il facem mai rapid:

```
with_fx :slicer, phase: 0.125 do
  synth :prophet, note: :e1, release: 8, cutoff: 70
  synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
end
```

Acum, joaca-te cu diferite durate pentru 'phase:'. Incearca valori mai mari sau mai mici. Vezi ce se intampla cand alegi o durata foarte scurta. De asemenea, incearca diferite sintetizatoare, cum ar fi ':beep' sau ':dsaw' si note diferite. Priveste diagrama urmatoare pentru a vedea cum schimba diferitele valori pentru ':phase' numarul de schimbari de amplitudine intr-un beat.

![Duratele fazei](../../../etc/doc/images/tutorial/articles/A.14-amplitude-modulation/slicer_phase_durations.png)

Durata fazei este timpul pentru un ciclu pornit/oprit. Ca urmare, valori mai mici vor face efectul sa comute mai rapid decat valori mai mari. Valori cu care ai putea experimenta sunt  `0.125`, `0.25`, `0.5` si `1`.


## Forma de unda de control

Implicit, efectul ':slicer' foloseste o forma de unda dreptunghiulara pentru a modifica amplitudinea in timp. De acceea auzim sunetul o perioada, apoi imediat dispare, apoi apare din nou. Forma de unda dreptunghiulara este doar una dintre cele 4 care pot fi folosite de catre ':slicer'. Celelalte sunt tip dinti de fierastrau, triunghiulara si (co)sinusoidala. Priveste diagrama de mai jos pentru a vedea cum arata diecare. Putem sa si auzim cum suna fiecare. De exemplu, codul de mai jos foloseste forma de unda co(sinusoidala) pentru control. Observi ca sunetul nu se opreste/porneste brusc ci are un efect de fade out/fade in:

```
with_fx :slicer, phase: 0.5, wave: 3 do
  synth :dsaw, note: :e3, release: 8, cutoff: 120
  synth :dsaw, note: :e2, release: 8, cutoff: 100
end
```

Joaca-te cu diferite forme de unda schimband parametrul 'wave:' la '0' pentru dinti de fierastrau, '1' pentru patrata, '2' pentru triunghiulara si '3' pentru sinusoidala. Observa de asemenea cum suna diferitele forme de unda cu valori diferite pentru 'phase:'.

Fiecare dintre aceste forme de unda poate fi inversata folosind parametrul 'invert_wave:' care o oglindeste fata de axa y. De exemplu, pe parcursul unei singure faze unda dinti de fierastrau porneste in general de sus si coboara incet pentru ca apoi sa sara instantaneu din nou la valoarea maxima. Folosind 'invert_wave: 1', ea va porni de jos si va creste progresiv inainte de a reveni instantaneu la valoarea minima. In plus, forma de unda poate porni din diferite puncte, folosind parametrul 'phase_offset:' care trebuie sa aiba o valoare intre '0' si '1'. Jucandu-te cu parametrii  `phase:`, `wave:`, `invert_wave:` si `phase_offset` poti schimba semnificativ modul in care amplitudinea este modificata in timp.

![Duratele fazelor](../../../etc/doc/images/tutorial/articles/A.14-amplitude-modulation/slicer_control_waves.png)


## Stabilirea nivelului

Implicit, ':slicer' comuta intre valorile '1' (sunet la maximum) si '0' (oprit complet) pentru amplitudine. Acestea pot fi schimbate folosind parametrii `amp_min:` si `amp_max:`. Poti folosi acesti parametri impreuna cu o unda sinusoidala pentru a crea un efect simplu de tremolo:

```
with_fx :slicer, amp_min: 0.25, amp_max: 0.75, wave: 3, phase: 0.25 do
  synth :saw, release: 8
end
```

Este ca si cum ai roti putin la stanga si la dreapta butonul de volum de la amplificatorul tau audio pentru ca taria sunetului sa oscileze putin.


## Probabilitati

Una dintre caracteristicile importante ale efectului ':slicer' este posibilitatea de a folosi probabilitatile pentru a alege daca activeaza sau nu modificarea de amplitudine. Inainte ca ':slicer' sa inceapa o noua faza, el da cu zarul si pe baza rezultatului fie foloseste forma de unda selectata fie mentine amplitudinea la 0. Sa ascultam:

```
with_fx :slicer, phase: 0.125, probability: 0.6  do
  synth :tb303, note: :e1, cutoff_attack: 8, release: 8
  synth :tb303, note: :e2, cutoff_attack: 4, release: 8
  synth :tb303, note: :e3, cutoff_attack: 2, release: 8
end
```

Observi ca acum avem un ritm interesant de impulsuri. Incearca sa setezi parametrul "probability:' la o valoare intre '0' si '1'. Valorile mai apropiate de 0 vor duce la pauze mai mari intre sunete deoarece probabilitatea ca sunetul sa fie redat este mult mai mica.

Un alt lucru de remarcat este ca sistemul de probabilitati din acest efect este la fel ca sistemul de randomizare accesibil prin intermediul functiilor ca 'rand' si 'shuffle'. Ambele sunt complet deterministe. Aceasta inseamna ca de fiecare data cand apesi pe Run vei auzi exact acelasi ritm de impulsuri pentru o probabilitate data. Daca vrei sa schimbi acest lucru poti folosi parametrul 'seed:' pentru a selecta un punct de pornire diferit in secventa de numere aleatoare. Acest parametru functioneaza exact ca `use_random_seed`, dar afecteaza doar acest efect particular.

In fine, poti schimba pozitia de "repaus" a undei de control pentru cazul in care testul probabilistic esueaza (deci nu se initiaza modificarea amplitudinii conform formei de unda) de la '0' la orice alta valoare, folosind parametrul 'prob_pos:':

```
with_fx :slicer, phase: 0.125, probability: 0.6, prob_pos: 1  do
  synth :tb303, note: :e1, cutoff_attack: 8, release: 8
  synth :tb303, note: :e2, cutoff_attack: 4, release: 8
  synth :tb303, note: :e3, cutoff_attack: 2, release: 8
end
```

## Crearea de beat-uri folosind slicing

Un lucru cu adevarat amuzant este sa folosesti ':slicer' pentru a cioparti un beat de toba:

```
with_fx :slicer, phase: 0.125 do
  sample :loop_mika
end
```

Acest lucru ne permite sa luam orice esantion si sa cream noi ritmuri, ceea ce este chiar distractiv. Totusi, un lucru de care trebuie sa tinem seama este ca tempo-ul esantionului trebuie sa fie acelasi cu BPM curent din Sonic Pi, altfel slicing-ul va suna total anapoda. De exemplu, incearca sa schimbi `:loop_mika` cu `loop_amen` pentru a auzi cat de rau suna cand tempo-urile nu coincid.

## Schimbarea tempo-ului

Dupa cum am vazut, modificarea BPM implicit cu 'use_bpm' va faca ca toate pauzele sau duratele anvelopelor pentru esantioane sa creasca sau sa scada pentru a se potrivi cu beat-ul. Efectul ':slicer' respecta aceeasi regula, parametrul 'phase:' fiind de fapt masurat in masuri, nu in secunde. Putem deci corecta problema de mai sus cu 'loop_amen' schimband BPM pentru a se potrivi cu cel al esantionului:

```
use_sample_bpm :loop_amen
with_fx :slicer, phase: 0.125 do
  sample :loop_amen
end
```

## Sa punem totul cap la cap

Sa aplicam toate aceste idei intr-un exemplu final care foloseste doar efectul ':slicer' pentru a crea o combinatie interesanta. Da-i drumul, modifica-l si transforma-l in propria ta piesa!

```
live_loop :dark_mist do
  co = (line 70, 130, steps: 8).tick
  with_fx :slicer, probability: 0.7, prob_pos: 1 do
    synth :prophet, note: :e1, release: 8, cutoff: co
  end
  
  with_fx :slicer, phase: [0.125, 0.25].choose do
    sample :guit_em9, rate: 0.5
  end
  sleep 8
end
live_loop :crashing_waves do
  with_fx :slicer, wave: 0, phase: 0.25 do
    sample :loop_mika, rate: 0.5
  end
  sleep 16
end
```




