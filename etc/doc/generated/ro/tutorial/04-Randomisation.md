4 Randomizare

# Randomizare

O modalitate de a face muzica ta mai interesanta este sa adaugi niste numere aleatoare (random). Sonic Pi ofera functii puternice pentru adaugarea unui caracter aleatoriu muzicii, dar inainte de a incepe trebuie sa aflam un lucru socant: in Sonic Pi *aleator nu inseamna cu adevarat aleator*. Ce naiba mai inseamna si asa? Ei bine, sa vedem.

## Repetabilitate

O functie cu adevarat utila este 'rrand', care va intoarce o valoare aleatoare intre doua numere - un *min* si un *max*. ('rrand' este prescurtarea de la ranged random - aleator intr-un interval). Sa incercam sa redam o nota aleatoare:

```
play rrand(50, 95)
```

O, a cantat o nota aleatoare. A cantat nota '83.7527'. O nota draguta aleasa aleator intre 50 si 95. Hei, stai asa, tocmai am prezis nota exacta pe care ai obtinut-o si tu? Ceva nu e in regula aici. Incearca sa mai executi codul o data. Ce? A ales `83.7527` din nou? Asta nu poate fi aleator!

Raspunsul este ca nu este cu adevarat aleator, ci pseudo-aleator. Sonic Pi va genera numere aparent aleatoare intr-un mode repetabil. Acest lucru este util daca vrei sa te asiguri ca muzica pe care ai creat-o pe calculatorul tau va suna identica pe oricare alt calculator - chiar si atunci cand ai introdus un pic de aleatorism in compozitia ta.

Desigur, intr-o piesa oarecare, daca alege mereu 'aleator' valoarea  `83.7527`, nu este prea interesant. Totusi, nu se intampla asa. Incearca asta:

```
live_loop :flibble do
  sample :bd_haus, rate: 1
  sleep 0.5
end 
```

Da! In fine, suna aleator. In cadrul unei executii a codului, apelurile succesive ale functiilor vor intoarce valori aleatoare. Totusi, la urmatoarea rulare vor fi generate aceleasi numere aleatoare si sunetul va fi exact la fel. Este ca si cand Sonic Pi s-ar intoarce in timp exact in acelasi punct de fiecare data cand apesi butonu Executa (Run). Este Ziua Cartitei pentru compozitia muzicala!

## Clopotele bantuite

Un ilustrare draguta a aleatorismului este exemplul cu clopotele bantuite care reda in bucla esantionul `:perc_bell` folosind valori aleatoare pentru viteza si pentru durata pauzelor intre sunete:

```
loop do
  sample :perc_bell, rate: (rrand 0.125, 1.5)
  sleep rrand(0.2, 2)
end
```

## Intrerupere (cutoff) aleatoare

Un alt exemplu de utilizare a randomizarii este modificarea aleatoare a momentului de cutoff pentru un sintetizator. Un sintetizator potrivit pentru a incerca asta este emulatorul ':tb303':

```
```
live_loop :flibble do
  sample :bd_haus, rate: 1
  sleep 0.5
end
```
```

## Valoarea de initializare (seed) a secventei aleatoare

Deci, ce faci faca nu-ti place o anumita secventa de numere aleatoare produsa de Sonic Pi? Ei bine, este posibil sa alegi un alt punct de pornire folosind 'use_random_seed'. Valoarea implicita este 0, deci alege o alta valoare pentru seed si vei avea o secventa aleatoare diferita!

Priveste aceasta bucata de cod:

```
5.times do
  play rrand(50, 100)
  sleep 0.5
end
```

De fiecare data cand vei exexuta acest cod, vei auzi aceeasi secventa de 5 note. Pentru a obtine o secventa diferita schimba valoarea pentru seed:

```
use_random_seed 40
5.times do
  play rrand(50, 100)
  sleep 0.5
end
```

Aceasta va produce o alta secventa de 5 note. Schimband valoarea de initializare si ascultand rezultatul poti gasi ceva ce iti place - iar cand vei transmite altora piesa, ei vor auzi exact acelasi lucru pe care l-ai auzit si tu.

Sa aruncam o privire si asupra altor functii utile pentru numere aleatoare.


## choose (alege)

O situatie des intalnita este cea in care vrem sa alegem un element la intamplare dintr-o lista predefinita. De exemplu, vreau sa redau o nota dintre urmatoarele: 60, 65 sau 72. Poti face acest lucru folosind 'choose' care imi permite sa aleg un element dintr-o lista. Mai intai, trebuie sa trec numerele intr-o lista, introducandu-le intre paranteze patrate si separandu-le cu virgule: '[60, 65, 72]'. Apoi le transmit functiei 'choose':

```
choose([60, 65, 72])
```

Sa ascultam sunetul obtinut:

```
loop do
  play choose([60, 65, 72])
  sleep 1
end
```

## rrand

Am discutat deja despre functia 'rrand', dar sa ne mai oprim putin asupra ei. Functia intoarce un numar aleator dintr-un interval deschis. Asta inseamna ca nu va intoarce niciodata valoarea minima sau pe cea maxima - intotdeauna va fi un numar intre cele doua. Numarul va fi intotdeauna de tipul float (cu virgula mobila) - insemnand ca nu este un numar intreg, ci are si o parte zecimala. Exemple de numere de tip float returnate de 'rrand(20, 110)':

* 87.5054931640625
* 86.05255126953125
* 61.77825927734375

## rrand_i

Vor fi si situatii in care vei dori un numar aleator intreg, nu unul cu virgula. Aici intervine 'rrand_i'. Functioneaza la fel ca 'rrand', doar ca poate returna si valorile minima sau maxima (foloseste un interval inchis de valori). Exemple de numere returnate de `rrand_i(20, 110)`:

* 88
* 86
* 62

## rand

Va returna un numar tip float intre 0 (inclusiv) si valoarea maxima pe care o specifici (exclusiv). Implicit va returna o valoare intre 0 si 1. Este deci util pentru alegerea aleatoare a valorilor pentru 'amp:':

```
loop do
  play 60, amp: rand
  sleep 0.25
end
```

## rand_i

Asemanator cu 'rrand_i' pentru 'rrand', exista 'rand_i' care va returna o valoare intreaga intre 0 si valoarea maxima pe care i-o transmiti.

## dice

Uneori vrei sa simulezi ca dai cu zarul - este un caz particular pentru 'rrand_i' in care valoarea minima este mereu 1. Un apel al functiei 'dice' cere sa specifici numarul de fete ale zarului. Un zar standard are 6 fete, deci 'dice(6)' va avea un comportament asemanator - va returna valorile 1, 2, 3, 4, 5 sau 6. Totusi, la fel ca in jocurile fantasy ai putea considera util un zar cu 4 fete, sau cu 12, sau 20 - poate chiar cu 120 de fete!

## one_in

In fine, poate doresti sa emulezi obtinerea scorului maxim la un zar, cum ar fi 6 in cazul celui standard. 'one_in' returneaza true cu o probabilitate de 1 din numarul de fete ale zarului. Astfel, 'one_in(6)' va returna true cu probabilitate de 1 din 6 si false in celelalte cazuri. True si false (adevarat si fals) sunt valori utile pentru instructiunile 'if' (daca) despre care vom vorbi mai tarziu in acest tutorial.

Acum, poti incepe sa zapacesti codul tau cu putin aleatorism!
