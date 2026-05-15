A.13 Programeaza un Sequencer probabilistic

# Programeaza un Sequencer probabilistic

Intr-un episod precedent am explorat randomizarea pentru a introduce varietate, surpriza si schimbare in interpretarile bazate pe programarea live. De exemplu, am ales la intamplare note dintr-o scala pentru a crea melodii fara sfarsit. Azi vom invata o tehnica noua care foloseste randomizarea pentru ritm - beat-uri probabilistice!

## Probabilitati

Inainte sa incepem sa cream noi beat-uri si ritmuri de sintetizator, trebuie sa discutam putin despre bazele probabilitatilor. Poate suna putin complicat, chiar infricosator, dar este la fel de simplu ca a da cu zarul, serios! Cand iei un zar obisnuit cu 6 fete si il rostogolesti, ce se intampla de fapt? Ei bine, in primul rand vei obtine un 1, 2, 3, 4, 5 sau 6, fiecare dintre acestea avand aceeasi sansa sa apara. De fapt, tinand cont de faptul ca zarul are 6 fete, vei obtine in medie un 1 la fiecare 6 aruncari (daca arunci zarul de un numar de ori suficient de mare). Asta inseamana ca ai o sansa din sase sa abtii un 1. Putem simula datul cu zarul in Sonic Pi folosind functia 'dice'. Sa dam cu zarul de 8 ori:

```
8.times do
  puts dice
  sleep 1
end
```

Observi cum in log apar valori intre 1 si 6, ca si cum am fi dat cu zarul.

## Beat-uri aleatoare

Acum sa ne imaginam ca ai o toba si de fiecare data cand te pregatesti sa o lovesti dai cu zatul. Daca obtii 1, lovesti toba, iar daca obtii altceva, nu o lovesti. Acum ai o toba automata care merge cu probabilitatea 1/6! Sa ascultam sunetul obtinut:

```
live_loop :random_beat do
  sample :drum_snare_hard if dice == 1
  sleep 0.125
end
```


Sa parcurgem rapid fiecare linie ca sa ne asiguram ca totul este clar. Mai intai cream o noua bucla live numita ':random_beat' care va repeta in continuu cele doua linii dintre 'do' si 'end'. Prima dintre aceste linii este un apel 'sample' care va reda un sunet pre-inregistrat (':drum_snare_hard' in cazul acesta). Aceasta linie are insa o conditie la sfarsit, introdusa de un 'if' (daca). Aceasta inseamna ca linia se va executa doar daca acea conditie care urmeaza dupa 'if' este indeplinita. Conditia in acest caz este 'dice == 1'. Este apelata functia 'dice' care, asa cum am vazut, returneaza o valoare intre 1 si 6. Apoi folosim operatorul '==' pentru a verifica daca aceasta valoare este '1'. Daca este '1', atunci conditia este indeplinita (are valoarea 'true', adica adevarat) si este redat sunetul de toba mica, daca nu este '1' conditia nu este indeplinita si instructiunea sunetul nu este redat. A doua linie introduce o pauza simpla de '0.125' secunde inainte de a da din nou cu zarul.

## Schimbarea probabilitatilor

Cei care ati jucat jocuri de tip RPG (role playing games) sunteti probabil familiari cu zarurile cu forme ciudate si cu numar diferit de fete. De exemplu, exista un zar in forma de tetraedru care are 4 fete, dar si zaruri cu 20 de fete in forma de icosaedru. Numarul de fete ale zarului schimba probabilitatea de a obtine un 1. Cu cat sunt mai putine fete, cu atat e mai probabil sa obtii un 1, iar cu cat sunt mai multe, cu atat este mai putin probabil.
De exemplu, pentru un zar cu 4 fete ai 1 sansa din 4, iar pentru zarul cu 20 de fete ai 1 sansa din 20. Din fericire, Sonic Pi are o functie 'one_in' foarte la indemana pentru a descrie exact acest lucru. Sa ne jucam:

```
live_loop :different_probabilities do
  sample :drum_snare_hard if one_in(6)
  sleep 0.125
end
```

Incepe cu bucla live de mai sus si vei auzi ritmul aleator deja cunoscut. Nu opri insa codul care ruleaza. In schimb, modifica '6' cu o valoare diferita, cum ar fi '2' sau '20' si apasa din nou butonul 'Run'. Observi ca un numar mai mic inseamna ca sunetul de toba mica se va auzi mai des, iar un numar mai mare inseamna ca sunetul se va auzi mai rar. Acum creezi muzica folosind probabilitatile!

## Combinarea probabilitatilor

Lucrurile pot deveni cu adevarat interesante cand combini mai multe esantioane care sunt redate cu probabilitati diferite. De exemplu:

```
live_loop :multi_beat do
  sample :elec_hi_snare if one_in(6)
  sample :drum_cymbal_closed if one_in(2)
  sample :drum_cymbal_pedal if one_in(3)
  sample :bd_haus if one_in(4)
  sleep 0.125
end
```

Executa codul de mai sus si apoi schimba probabilitatile pentru a modifica ritmul. De asemenea, incerca sa schimbi esantioanele pentru a crea ceva cu totul diferit. De exemplu, schimba `:drum_cymbal_closed` cu `:bass_hit_c` pentru mai mult bas!


## Ritmuri repetabile

In continuare, putem apela la vechiul nostru prieten 'use_random_seed' pentru a reseta sirul aleator dupa fiecare 8 iteratii pentru a crea un beat regulat. Scrie codul de mai jos pentru a auzi un ritm mai regulat care se repeta. Dupa ce se aude beat-ul, incearca sa schimbi valoarea de initializare (random seed) din 1000 in altceva. Observi ca numere diferite vor produce beat-uri diferite.

```
live_loop :multi_beat do
  use_random_seed 1000
  8.times do
    sample :elec_hi_snare if one_in(6)
    sample :drum_cymbal_closed if one_in(2)
    sample :drum_cymbal_pedal if one_in(3)
    sample :bd_haus if one_in(4)
    sleep 0.125
  end
end
```

Un lucru pe care il fac de obicei cu acest tip de structura este sa vad ce valoare de initializare suna mai bine si sa o notez. In felul asta pot recrea usor ritmurile mele in viitor cand exersez sau interpretez live.

## Sa punem totul cap la cap

In fine, putem adauga niste bas aleator pentru a crea un continut melodic placut. Putem folosi metoda probabilistica de redare a esantioanelor pe care tocmai am descoperit-o. Dar nu lasa lucrurile asa - modifica numerele si creeaza propria ta melodie folosind puterea probabilitatilor!

```
live_loop :multi_beat do
  use_random_seed 2000
  8.times do
    c = rrand(70, 130)
    n = (scale :e1, :minor_pentatonic).take(3).choose
    synth :tb303, note: n, release: 0.1, cutoff: c if rand < 0.9
    sample :elec_hi_snare if one_in(6)
    sample :drum_cymbal_closed if one_in(2)
    sample :drum_cymbal_pedal if one_in(3)
    sample :bd_haus, amp: 1.5 if one_in(4)
    sleep 0.125
  end
end
```
