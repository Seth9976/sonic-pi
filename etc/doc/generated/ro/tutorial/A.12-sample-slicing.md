A.12 Slicing cu esantioane

# Slicing cu esantioane

In episodul 3 al acestei serii despre Sonic Pi am vazut cum putem reda in bucla, intinde sau comprima si filtra unul dintre cele mai faimoase solo-uri de toba din toate timpurile - Amen Break. In acest tutorial o sa mergem cu un pas mai departe si o sa vedem cum putem sa il maruntim (slice), sa amestecam bucatile si sa le combinam la loc in alta ordine. Daca ti se pare o chestie putin traznita, nu te ingrijora, lucrurile vor deveni mai clare in curand si vei stapani o noua unealta puternica pentru interpretarile live folosind Sonic Pi.

## Sunetul ca date

Inainte sa incepem, sa explicam putin cum functioneaza esantioanele. Pana acum sper ca te-ai jucat deja cu sampler-ul din Sonic Pi. Daca nu, nu exista un moment mai potrivit decat acesta! Porneste Raspberry Pi, lanseaza Sonic Pi din meniul Programming, scrie codul urmator intr-un buffer gol si apasa butonul Run pentru a auzi un beat de toba pre-inregistrat:

```
sample :loop_amen
```

O inregistrare a unui sunet este reprezentata sub forma de date - o multime de numere intre -1 si 1 care reprezinta limitele undei sonore. Daca redam aceste numere in ordine, obtinem sunetul original. Dar ce ne opreste sa le redam intr-o alta ordine, pentru a crea alte sunete?

Cum sunt inregistrate de fapt esantioanele? O sa ti se para simplu dupa ce intelegi bazale fizicii sunetului. Cand produci un sunet, de exemplu lovind in toba, acesta se propaga prin aer la fel cum se increteste suprafata apei cand arunci o piatra in ea. Cand aceste unduiri ajung la urechea noastra, timpanul se misca in ritm cu ele si converteste aceste miscari in sunetul pe care il auzim. Daca dorim sa inregistram si sa redam sunetul, trebuie deci sa capturam, sa stocam si sa reproducem aceste unduiri. O metoda este sa folosim un microfon care actioneaza ca un timpan si se misca inainte si inapoi cand unda ajunge la el. Microfonul converteste apoi pozitia respectiva intr-un semnal electric care este masurat de multe ori intr-o secunda. Aceste masuratori sunt reprezentate sub forma unei serii de numere intre -1 si 1.

Daca am incerca sa gasim o reprezentare vizuala a sunetului ar fi un grafic simplu cu timpul pe axa x si pozitia microfonului/difuzorului pe axa y, avand valori intre -1 si 1. Poti vede un exemplu de astfel de grafic in partea de sus a diagramei.

## Redarea unei parti dintr-un esantion

Cum procedam deci pentru a cere Sonic Pi sa redea un esantion intr-o ordine diferita. Pentru a raspunde la aceasta intrebare trebuie sa analizam parametrii 'start:' si 'finish:' pentru 'sample'. Acestia ne permit sa controlam pozitia de inceput si de final pentru numerele reprezentand esantionul care vor fi redate. Valorile pentru ambii parametri sunt reprezentate de numere intre '0' (inceputul esantionului) si '1' (sfarsitul sau). Deci, pentru a reda prima jumatate din Amen Break, trebuie sa setam valoarea '0.5' pentru 'finish:':

```
sample :loop_amen, finish: 0.5
```

Putem adauga si o valoare pentru 'start:' ca sa redam o sectiune si mai mica din esantion:

```
sample :loop_amen, start: 0.25, finish: 0.5
```

Ca sa te distrezi, poti alege o valoare *mai mica* pentru 'finish:' decat pentru 'start:' si respectiva sectiune va fi redata invers:

```
sample :loop_amen, start: 0.5, finish: 0.25
```

## Schimabarea ordinii de redare a esantionului

Acum ca stim ca un esantion este doar o lista de numere care pot fi redate in orice ordine si stim si cum sa redam doar o anumita parte din esantion, putem incepe sa ne distram redand un esantion in ordine 'gresita'.

![Slicing Amen](../../../etc/doc/images/tutorial/articles/A.12-sample-slicing/amen_slice.png)

Sa ne uitam putin la Amen Break si sa il taiem in 8 felii egale apoi sa le amestecam. Priveste diagrama: sus A) reprezinta graficul datelor din esantionul initial. Taierea lui in 8 bucati ne da B) - observi ca am colorat diferit fiecare dintre cele 8 felii pentru a le distinge mai usor. Deasupra poti vedea valorile de inceput si de sfarsit pentru fiecare felie. In fine, C) reprezinta una dintre re-aranjarile posibile pentru cele 8 felii. Putem reda aceste numere pentru a crea un nou beat. Sa privim codul care face acest lucru:

```
live_loop :beat_slicer do
  slice_idx = rand_i(8)
  slice_size = 0.125
  s = slice_idx * slice_size
  f = s + slice_size
  sample :loop_amen, start: s, finish: f
  sleep sample_duration :loop_amen, start: s, finish: f
end
```

1. Alegem aleator o felie pentru redare folosind un numar aleator intre 0 si 7 (adu-ti aminte ca incepem numerotarea de la 0). Sonic Pi are o functie foarte utila pentru acest lucru: 'rand_i(8)'. Memoram apoi acest index aleator in variabila 'slice_idx'.
2. Definim marimea feliei - 'slice_size' ca fiind 1/8 sau 0.125. Variabila 'slice_size' ne este utila pentru a converti 'slice_idx' intr-o valoare intre 0 si 1, ca sa o putem folosi pentru parametrul 'start:'.
3. Calculam pozitia de start 's' inmultind 'slice_idx' cu 'slice_size'.
4. Calculam pozitia de final 'f' adaugand 'slice_size' la valoarea pozitiei de start 's'.
5. Acum putem reda felia din esantion transmitand 's' si 'f' ca valori pentru parametrii 'start:' si 'finish:' cand apelam 'sample'.
6. Inainte sa redam urmatoarea felie trebuie sa stim cat de mult trebuie sa dureze pauza, care trebuie sa fie la fel de lunga ca durata unei felii din esantion. Din fericire, Sonic Pi ne ofera functia 'sample_duration' care accepta aceiasi parametri ca 'sample' si returneaza durata. Apeland deci 'sample_duration' cu parametri 'start:' si 'finish:', putem afla durata unei felii de esantion.
7. Introducem toate acestea intr-o bucla live ca sa putem alege in continuu noi felii aleatoare pentru redare.


## Sa punem totul cap la cap

Sa punem cap la cap tot ce am vazut pana acum intr-un exemplu final care arata cum putem folosi o abordare asemanatoare pentru a combina beat-uri sectionate aleator (sliced) cu niste bas pentru a obtine punctul de pornire pentru o pista interesanta. Acum este randul tau - ia codul de mai jos si modifica-l cum iti place tie pentru a crea ceva nou...

```
live_loop :sliced_amen do
  n = 8
  s =  line(0, 1, steps: n).choose
  f = s + (1.0 / n)
  sample :loop_amen, beat_stretch: 2, start: s, finish: f
  sleep 2.0  / n
end
live_loop :acid_bass do
  with_fx :reverb, room: 1, reps: 32, amp: 0.6 do
    tick
    n = (octs :e0, 3).look - (knit 0, 3 * 8, -4, 3 * 8).look
    co = rrand(70, 110)
    synth :beep, note: n + 36, release: 0.1, wave: 0, cutoff: co
    synth :tb303, note: n, release: 0.2, wave: 0, cutoff: co
    sleep (ring 0.125, 0.25).look
  end
end
```
