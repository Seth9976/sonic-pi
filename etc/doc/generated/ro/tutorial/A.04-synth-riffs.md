A.4 Riff-uri (fraze melodice) pentru sintetizatoare

# Riff-uri (fraze melodice) pentru sintetizatoare

Fie ca este vorba de sunetul obsedant al unui oscilator sau de punch-ul unei unde in dinti de fierastrau care razbate din mix, sintetizatorul principal are un rol esential in orice piesa electronica. Luna trecuta am vorbit in acest tutorial despre cum putem coda un beat. Acum vom vorbi despre cum putem coda cele trei componente de baza ale riff-ului pentru sintetizator: timbrul, melodia si ritmul.

Asadar, porneste Raspberry Pi, deschide Sonic Pi v2.6+ si sa facem galagie!


## Controlul timbrului

O parte esentiala a unui riff pentru sintetizator este modificarea timbrului sunetelor. Putem controla timbrul in Sonic Pi in doua feluri - alegand sintetizatoare diferite pentru o schimbare masiva sau modificand diferiti parametri ai sintetizatorului pentru schimbari mai subtile. Putem folosi si efectele, dar acesta este subiectul altui tutorial...

Sa cream o bucla live simple in care schimbam in continuu sintetizatorul curent:

```
live_loop :timbre do
  use_synth (ring :tb303, :blade, :prophet, :saw, :beep, :tri).tick
  play :e2, attack: 0, release: 0.5, cutoff: 100
  sleep 0.5
end
```

Arunca o privire asupra codului. Pur si simplu parcurgem ritmic o lista circulara de nume de sintetizatoare (fiecare dintre acestea va fi selectat pe rand, reluand lista de la inceput cand se termina).  Acest nume de sintetizator este transmis functiei 'use_synth' care schimba sintetizatorul curent din bucla live. De asemenea, redam nota ':e2' (E din a doua octava), cu un timp de release de 0.5 batai (jumatate de secunda la viteza implicita de 60 BPM) si cu un 'cutoff:' setat la 100.

Poti auzi cum diferite sintetizatoare au sunete foarte diferite chiar daca redau toate aceeasi nota. Acum experimenteaza si distreaza-te. Schimba timpul de release cu valori mai mici sau mai mari. Schimba parametrii 'attack:' si 'release:' pentru a vedea cum timpi diferiti de fade in/fade out pot avea un impact foarte mare asupra sunetului. Apoi schimba valoarea pentru 'cutoff:' si vei vede ca are o influenta foarte importanta asupra timbrului (iti recomand valori intre 60 si 130). Vei vedea cat de multe sunete diferite poti crea schimband doar cateva valori. Odata ce stapanesti aceste schimbari mergi la panoul Sintetizatoare (Synths) din sistemul de ajutor pentru o lista completa a sintetizatoarelor si a tuturor parametrilor pe care ii suporta fiecare dintre acestea pentru a vedea ce puteri magice poti controla cu degetele tale de programator.

## Timbrul

Timbru este un nume pompos pentru un cuvant care descrie sunetul unui sunet. Daca redai aceeasi nota cu instrumente diferite, cum ar fi o vioara, o chitara sau un pian, inaltimea sunetului va fi aceeasi, dar calitatea sunetului va fi diferita. Aceasta calitate a sunetului - lucrul care iti permite sa sesizezi diferenta dintre un pian si o chitara este timbrul.


## Compozitia melodica

Un alt element important pentru sintetizatorul principal este alegerea notelor pe care vrem sa le redea. Daca ai deja o idee, poti sa creezi pur si simplu o lista circulare cu notele si sa o parcurgi pas cu pas:

```
live_loop :riff do
  use_synth :prophet
  riff = (ring :e3, :e3, :r, :g3, :r, :r, :r, :a3)
  play riff.tick, release: 0.5, cutoff: 80
  sleep 0.25
end
```
    
Aici am definit melodia printr-o lista care contine atat note, cum ar fi ':e3', cat si pauze, cum ar fi ':r'. Apoi folosim '.tick' pentru a parcurge toate notele ciclic, obtinand un riff care se repeta.

## Generarea automata a melodiei

Nu e mereu simplu sa produci un riff interesant pornind de la zero. E mai usor uneori sa ceri Sonic Pi o selectie de riff-uri aleatoare dintre care sa-l alegi pe cel care iti place cel mai mult. Pentru asta vom combina 3 lucruri: liste circulare, randomizarea si initializarea unui sir aleator folosind un seed. Sa vedem un exemplu:

```
live_loop :random_riff do
  use_synth :dsaw
  use_random_seed 3
  notes = (scale :e3, :minor_pentatonic).shuffle
  play notes.tick, release: 0.25, cutoff: 80
  sleep 0.25
end
```

Aici se petrec mai multe lucruri. Sa le luam pe rand. In primul rand am ales pentru initializarea sirului valoarea 3. Ce inseamna asta? Cand alegem valoarea de initializare (seed = samanta) putem prezice care va fi urmatoarea valoare aleatoare dintr-un sir - va fi la fel ca si ultima data cand am folosit numarul 3 pentru seed! Alt lucru de retinut este ca amestecarea notelor dintr-o lista functioneaza la fel. In exemplul de mai sus cerem de fapt 'a treia varianta de amestecare' in lista predefinita de variante - care va fi de asemenea la fel de fiecare data cand vom alege acelasi seed. La final, parcurgem ciclic notele amestecate pentru a canta riff-ul.

Acum incepe partea distractiva. Daca schimbam valoare pentru seed cu alta valoare, sa zicem 3000, obtinem o varianta complet diferita de amestecare a notelor. Deci este extrem de usor sa incercam noi melodii. Pur si simplu alegem lista de note pe care vrem sa le amestecam (gamele sunt un bun punct de pornire)  si apoi alegem seed-ul pentre amestecare. Daca nu ne place melodia, doar schimbam unul dintre aceste lucruri si incercam din nou. Repetam pana ne place ce auzim!


## Pseudo-randomizarea

Randomizarea in Sonic Pi nu da rezultate cu adevarat aleatoare, ci pseudo-aleatoare. Imagineaza-ti ca dai cu zarul de 100 de ori si scrii rezultatul de fiecare data pe o foaie de hartie. Sonic Pi foloseste ceva echivalent cu aceste liste de rezultate atunci cand ceri o valoare aleatoare. In loc sa dea cu zarul, doar alege urmatoarea valoare din lista. Alegerea seed-ului pentru sirul aleator inseamna deplasarea la o anumita pozitie din lista.
 
## Gaseste-ti ritmul

Alt element important al riff-ului este ritmul - cand a redai si cand sa nu redai o nota. Dupa cum am vazut mai sus, putem folosi ':r' in listele de note pentru a insera pauze. Alta metoda foarte versatila este folosirea spread-urilor pe care o vom prezenta intr-un tutorial viitor. Astazi vom folosi randomizarea care ne va ajuta sa ne gasim ritmul dorit. In loc sa redam fiecare nota, putem folosi o instructiune conditionala pentru a reda o nota cu probabilitatea aleasa. Sa aruncam o privire:

```
live_loop :random_riff do
  use_synth :dsaw
  use_random_seed 30
  notes = (scale :e3, :minor_pentatonic).shuffle
  16.times do
    play notes.tick, release: 0.2, cutoff: 90 if one_in(2)
    sleep 0.125
  end
end
```

O functie pe care este foarte util sa o stii este 'one_in' care va returna o valoare de adevar ('true' sau 'false') cu probabilitatea ceruta de noi. Aici folosim valoarea 2, deci in medie functia 'one_in' va returna 'true' o data la fiecare doua apeluri. Cu alte cuvinte, in 50% dintre cazuri va returna 'true'. Folosind valori mai mari, vom obtine rezultatul 'false' mai des, introducand mai multe pauza in riff.

Ai observat ca am adaugat o iteratie folosind '16.times'. Asta deoarece vrem sa resetam sirul pseudoaleator doar o data la fiecare 16 note, astfel incat ritmul nostru sa se repete de 16 ori. Aceasta nu influenteaza amestecarea notelor deoarecea aceasta este realizata imediat dupa ce valoarea de initializare pentru sirul plseudoaleator este stabilita. Putem folosi dimensiunea iteratiei pentru a modifica lungimea riff-ului. Schimba din 16 in 8 sau chiar 4 sau 3 pentru a vedea cum afecteaza ritmul riff-ului.

## Sa punem totul cap la cap

OK, sa punem cap la cap ce am invatat intr-un exemplu final. Ne vedem data viitoare!

```
live_loop :random_riff do
  #  uncomment to bring in:
  #  synth :blade, note: :e4, release: 4, cutoff: 100, amp: 1.5
  use_synth :dsaw
  use_random_seed 43
  notes = (scale :e3, :minor_pentatonic, num_octaves: 2).shuffle.take(8)
  8.times do
    play notes.tick, release: rand(0.5), cutoff: rrand(60, 130) if one_in(2)
    sleep 0.125
  end
end
 
live_loop :drums do
  use_random_seed 500
  16.times do
    sample :bd_haus, rate: 2, cutoff: 110 if rand < 0.35
    sleep 0.125
  end
end
 
live_loop :bd do
  sample :bd_haus, cutoff: 100, amp: 3
  sleep 0.5
end
```
