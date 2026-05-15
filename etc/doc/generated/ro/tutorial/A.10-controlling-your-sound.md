A.10 Controlul

# Controlul sunetului

Pana acum in aceasta serie de tutoriale ne-am concentrat pe pornirea sunetelor. Am descoperit ca putem porni un sintetizator din multele incluse in Sonic Pi folosind 'play' sau 'synth' sau un esantion pre-inregistrat folosind 'smple'. De asemenea am vazut cum putem trece aceste sunete prin efecte de studio ca reverb sau distorsion folosind comanda 'with_fx'. Combina toate aceste cu sistemul foarte precis de pozitionare in timp al Sonic Pi si poti produce o gama vasta de sunete, beat-uri si riff-uri. Dar dupa ce ai selectat cu grija optiunile pentru un anumit sunet si l-ai pornit, nu mai exista nicio posibilitate sa il modifici, nu? Gresit! Astazi vei descoperi o functionalitate foarte puternica - controlul sintetizatorului in timpul redarii.

## A Sunetul de baza

Sa cream un sunet simplu si dragut. Porneste Sonic Pi si scrie asta intr-un buffer gol:

```
synth :prophet, note: :e1, release: 8, cutoff: 100
```

Acum apasa butonul Run din stanga sus ca sa auzi un sunet dragut de sintetizator. Continua, mai apasa de cateva ori butonul ca sa incepi sa il simti. Gata? Sa incepem sa-l controlam!

## SynthNode

O caracteristica putin cunoscuta a Sonic Pi este aceea ca functiile 'play', 'synth' si 'sample' returneaza ceea ce se numeste un 'SynthNode' care reprezinta un sunet in curs de redare. Poti memora un astfel de 'SynthNode' intr-o variabila si apoi sa il **controlezi** mai tarziu. De exemplu, sa schimbam valoarea pentru parametrul 'cutoff:' dupa o masura:

```
sn = synth :prophet, note: :e1, release: 8, cutoff: 100
sleep 1
control sn, cutoff: 130
```

Sa privim fiecare linie pe rand:

Mai intai lansam sintetizatorul ':propher' folosind functia 'synth' ca de obicei. Dar in acelasi timp memoram si rezultatul intr-o variabila numita 'sn'. Puteam sa-i dam orice alt nume variabilie, cum ar fi 'synth_node' sau 'jane' - numele nu conteaza. Totusi, este important sa alegi un nume care iti spune ceva pentru cand interpretezi live se pentru alte persoane care ar putea citi codul tau. E am ales 'sn' pentru ca este o prescurtare draguta pentru SynthNode.

Pe linia a doua avem o comanda 'sleep' obisnuita. Nu face nimic special, doar ii cere computerului sa astepte o masura inainte sa treaca la linia urmatoare.

Linia 3 este cea in care se intampla totul. Aici am folosit functia 'control' pentru a spune sintetizatorului sa schimbe valoarea de cutoff in '130' in timp ce ruleaza. Daca apesi butonul **Run**, vei auzi cum sintetizatorul ':prophet' incepe sa cante ca mai inainte, dar dupa o masura va incepe sa sune mult mai clar.

Parametri modulabili

Multi dintre parametrii pentru sintetizatoarele si efectele din Sonic Pi pot fi schimbati dupa ce au fost pornite. Nu este insa cazul pentru toate. De exemplu, parametrii anvelopei `attack:`, `decay:`, `sustain:` si `release:` pot fi specificati doar cand se porneste sintetizatorul. E destul de usor sa afli care parametri pot si care nu pot fi schimbati - mergi la documentatia unui anumit sintetizator sau efect si deruleaza in jos pana la informatiile despre parametrii individuali si cauta fraza "Poate fi schimbat in timpul rularii", respectiv "Nu mai poate fi schimbat odata setat". De exemplu, documentatia pentru parametrul 'attack:' al sintetizatorului ':beep' spune clar ca nu este posibil sa il schimbi:

* Implicit: 0
* Trebuie sa fie zero sau mai mare
* Nu poate fi schimbat odata setat
* Modificat proportional cu viteza BPM

## Schimbari multiple

In timp ce un sintetizator ruleaza poti sa il schimbi de cate ori vrei. De exemplu, putem transforma sintetizatorul ':prophet' intr-un generator de arpegii folosind:

```
notes = (scale :e3, :minor_pentatonic)
sn = synth :prophet, note: :e1, release: 8, cutoff: 100
sleep 1
16.times do
  control sn, note: notes.tick
  sleep 0.125
end
```

In aceasta bucata de cod am adaugat cateva chestii in plus. Am definit o noua variabila, numita 'notes', care contine notele pe care vrem sa le reda in mod repetat (un generator de arpegii chiar asta inseamna - ceva ce reda notele dintr-o lista circulara in ordine). Apoi am inlocuit apelul individual al functiei 'control' cu o iteratie care o apeleaza de 16 ori. La fiecare apel al functiei folosim '.tick' pentru a parcurge lista 'notes' care se va repeta automat cand ajungem la sfarsit (gratie minunatelor liste circulare din Sonic Pi). Pentru putina varietate incearca sa inlocuiesti '.tick' cu '.choos' si vezi daca remarci diferenta.

Ai observat ca putem schimba mai multi parametri simultan. Incearca sa schimbi linia in care apare functia control cu aceasta si asculta diferentele:

```
control sn, note: notes.tick, cutoff: rrand(70, 130)
```

## Sliding

Cand controlam un 'SynthNode', el raspunde imediat si schimba instantaneu valoarea parametrului cu cea noua ca si cand ai fi apasat un buton sau ai fi intors de un comutator pentru a cere modificarea. Asta genereaza un sunet ritmic si percutant - mai ales daca parametrul controleaza ceva legat de timbru, cum ar fi 'cutoff:'. Totusi, uneoi nu vrei ca schimbarea sa se produca instantaneu. In schimb, ti-ai dori o trecere lina de la valoarea curenta la noua valoare ca si cand ai fi miscat un cursor. Desigur, Sonic Pi poate face si acest lucru, folosind parametrii '_slide'.

Fiecare parametru care poate fi modificat are asociat si un parametru '_slide:' care permite specificarea timpului in care se face modificarea graduala. De exemplu, pentru 'amp:' avem `amp_slide:`, iar pentru `cutoff:` avem `cutoff_slide:`. Acesti parametri functioneaza putin diferit de ceilalti prin aceea ca spun sintetizatorului cum sa actioneze **data viitoare cand primesc o comanda de control**. Sa aruncam o privire:

```
sn = synth :prophet, note: :e1, release: 8, cutoff: 70, cutoff_slide: 2
sleep 1
control sn, cutoff: 130
```

Observi ca exemplul este la fel cu cel dinainte cu exceptia faptului ca am adaugat `cutoff_slide:`. Aceasta face ca data viitoare cand i se cere sintetizatorului sa schimbe valoarea de cutoff, el sa faca trecea de la valoarea veche la cea noua gradual, pe durata a 2 masuri. Ca urmare, cand vei folosi 'control' vei auzi cum 'cutoff' trece lin de la 70 la 130, ceea ce da sunetului un aer dinamic. Acum incearca sa schimbi valoarea pentru 'cutoff_slide' cu una mai mica, cum ar fi 0.5 sau o valoare mai mare, cum ar fi 4, pentru a observa cum se modifica sunetul. Tine minte, in acelasi mod poti face sa varieze treptat orice parametru modificabil, iar fiecare valoare '_slide:' poate fi complet diferita astfel. De exemplu cutoff poate sa se schimbe lent, amplitudinea mai rapid, iar pozitionarea cu o viteza intre cele doua, daca asta iti doresti...

## Sa punem totul cap la cap

Sa analizam putin un scurt exemplu care ne arata puterea controlarii sintetizatoarelor dupa ce au fost pornite. Observi ca pot fi modificate treptat si efectele la fel ca sintetizatoarele, dar cu o sintaxa putin diferita. Citeste sectiunea 7.2 a tutorialului pentru mai multe informatii despre controlul efectelor.

Copiaza codul intr-un buffer gol si asculta. Nu te opri aici, joca-te putin cu codul. Schimba valorile pentru '_slide:', schimba notele, sintetizatorul, efectele si duratele pentru pauze si vezi cum poti sa schimbi sunetul in ceva cu totul diferit!

```
live_loop :moon_rise do
  with_fx :echo, mix: 0, mix_slide: 8 do |fx|
    control fx, mix: 1
    notes = (scale :e3, :minor_pentatonic, num_octaves: 2).shuffle
    sn = synth :prophet , sustain: 8, note: :e1, cutoff: 70, cutoff_slide: 8
    control sn, cutoff: 130
    sleep 2
    32.times do
      control sn, note: notes.tick, pan: rrand(-1, 1)
      sleep 0.125
    end
  end
end
```
