A.11 Tic Tac

# Urmarirea ritmului

In episodul de luna trecuta am facut o incursiune tehnica in sistemul de randomizare din Sonic Pi. Am explorat cum il putem folosi pentru a adauga un nou nivel de control dinamic in codul nostru intr-un mod determinist. Luna asta vom continua sa studiem detaliile tehnice si ne vom indrepta atentia asupra sistemului de tact unic in Sonic Pi. La sfarsitul acestui articol vei putea sa urmaresti comanzi ritmurile si riff-urile si vei fi cu un pas mai aproape sa devii un DJ care programeaza live.

## Numararea masurilor

Cand facem muzica ne dorim adesea sa facem lucruri diferite in functie de masura la care am ajuns. Sonic Pi are un sistem special de contorizare a masurilor, numit 'tick', care iti ofera un control precis asupra momentului in care incepe masura si suporta chiar mai multe tempo-uri in paralel.

Sa ne jucam putin, pentru a trece la masura urmatoare trebuie doar sa apelezi '.tick'. Deschide un buffer nou si scrie codul de mai jos apoi apasa Run:

```
puts tick #=> 0
```

Va fi afisata masura curenta: '0'. Poti observa ca daca apesi pe Run de mai multe ori, de fiecare data va returna '0'. Asta se intampla deoarece la fiecare Run numaratoarea masurilor incepe de la 0. In timpul rularii insa, putem avansa la urmatoarea masura de cate ori vrem:

```
puts tick #=> 0
puts tick #=> 1
puts tick #=> 2
```

De cate ori observi simbolul '#=>' la sfarsitul unei linii de cod, inseamna ca linia va determina scrierea in jurnal a partii care urmeaza. De exemplu, 'puts foo#=> 0' inseamna ca instructiunea 'puts foo' scrie '0' in jurnal la acel punct din program.

## Schimbarea masurii

Am vazut ca 'tick' face doua lucruri. Incrementeaza (creste cu 1) valoarea masurii curente si returneaza noua valoare. Uneori vrem doar sa vedem valoarea curenta, fara sa trecem la masura urmatoare, ceea ce poate fi realizat cu ajutorul 'look':

```
puts tick #=> 0
puts tick #=> 1
puts look #=> 1
puts look #=> 1
```

In acest cod generam un tact de 2 ori si afisam valoarea cu '.tick', apoi doar analizam valoarea de 2 ori cu '.look'. Vom vedea urmatoarele valori: `0`, `1`, `1`, `1`. Primele 2 utilizari ale functiei 'tick' returneaza '0' si apoi '1', cum era de astepta, apoi cele 2 functii 'look' returneaza ultima valoare care este '1'.


## Liste circulare

Deci acum putem trece la masura urmatoare folosind 'tick' si sa vedem unde am ajuns folosind 'look'. Ce urmeaza? Trebuie sa avem ceva ce poate fi parcurs pas cu pas. Sonic Pi foloseste liste circulare pentru a reprezenta riff-uri, melodii si ritmuri, iar sistemul de tact a fost gandit special pentru a lucra strans cu acestea. De fapt, listele circulare au propriile versiuni de tip sufix pentru functia 'tick', care realizeaza doua lucruri. In primul rand actioneaza ca un tick obisnuit si trec la masura urmatoare, iar in al doilea rand parcurg listele circulare folosind numarul masurii ca index. Sa aruncam o privire:

```
puts (ring :a, :b, :c).tick #=> :a
```

'.tick' este o versiune speciala de tip sufix a functiei '.tick', care va returna prima valoare din lista ':a'. Putem obtine fiecare dintre valorile din lista apeland '.tick' de mai multe ori:

```
puts (ring :a, :b, :c).tick #=> :a
puts (ring :a, :b, :c).tick #=> :b
puts (ring :a, :b, :c).tick #=> :c
puts (ring :a, :b, :c).tick #=> :a
puts look                   #=> 3
```

Urmareste jurnalul si vei vedea `:a`, `:b`, `:c` si iarasi `:a`. Observi ca 'look' returneaza valoarea '3'. Apelurile '.tick' functioneaza la fel ca cele ale functiei 'tick' obisnuite' - incrementeaza numarul masurii curente.


## Creator de arpegii intr-o bucla live

Adevarata distractie incepe cand combinam 'tick' cu listele circulare si cu buclele live. Cand punem aceste lucruri impreuna avem tot ce ne trebuie pentru a construi si a intelege un generatori simplu de arpegii. Avem nevoie de doar patru lucruri:

1. O lista circulara care contine notele pe care vrem sa le repetam.
2. O modalitate de a incrementa si de a obtine numarul masurii curente.
3. Capacitatea de a reda o nota care depinde de masura curenta.
4. O structura de tip bucla care face ca generatorul sa repete arpegiile.

Toate aceste concepte pot fi intalnite in codul urmator:

```
notes = (ring 57, 62, 55, 59, 64)
live_loop :arp do
  use_synth :dpulse
  play notes.tick, release: 0.2
  sleep 0.125
end
```

Sa privim fiecare dintre aceste linii. Mai intai definim lista de note pe care le vom reda in continuu. Apoi cream o bucla live numita ':arp' care se ocupa de repetarea instructiunilor pentru noi. De fiecare data cand intram in bucla algem sintetizatorul ':dpulse' si apoi redam urmatoarea nota din lista cu ajutorul '.tick'. Tine minte ca acest lucru va duce la incrementarea numarului masurii curente si va folosi aceasta valoare ca index in lista cu notele. La final asteptam o optime de masura inainte de a relua bucla.

## Ritmuri multiple

Un lucru important de stiut este ca tactul este local pentru o bucla live. Asta inseamna ca fiecare 'live_loop' are propriul contor pentru masura. Acest lucru ne ofera mai multa flexibilitate decat un metronom global si un tempo global. Sa vedem cum functioneaza:

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

## Conflicte de numarare

O sursa de confuzii si erori in utilizarea sistemului de tact din Sonic Pi este folosirea unui tick pentru mai multe liste in acelasi 'live_loop':

```
use_bpm 300
use_synth :blade
live_loop :foo do
  play (ring :e1, :e2, :e3).tick
  play (scale :e3, :minor_pentatonic).tick
  sleep 1
end
```

Fiecare 'live_loop' are propriul contor pentru masura, dar noi apelam '.tick' de doua ori in cadrul aceleiasi bucle. Asta inseamna ca numarul masurii curente va fi incrementat de doua ori la fiecare trecere. Uneori pot aparea sunete poliritmice interesante, dar nu asta ne dorim in general. Exista doua solutii pentru aceasta problema. O varianta ar fi sa apelam 'tick' la inceputul buclei live si apoi sa folosim 'look' pentru a vedea care este valoarea curenta. A doua ar fi sa transmitem un nume unic la fiecare dintre cele 2 apeluri ale functiei '.tick', cum ar fi '.tick(:foo)'. Sonic Pi va crea si va urmari un contor separat pentru fiecare nume pe care il folosesti. In felul asta poti lucra cu oricate tacturi ai nevoie. Vezi sectiunea 9.4 din tutorial pentru mai multe informatii despre folosirea de nume pentru tacturi.

## Sa punem totul cap la cap

Sa punem cap la cap cunostintele noastre despre tact, liste circulare si bucle live pentru un exemplu final. Ca de obicei, nu trebuie sa tratezi asta ca pe o bucata finisata. Poti schimba ce doresti ca sa vezi in ce o poti transforma. Ne vedem data viitoare...

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
