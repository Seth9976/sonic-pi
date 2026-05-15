A. Basul acid

# Basul acid

Este imposibil sa treci in revista istoria muzicii dance electronice fara sa remarci impactul major al micutului sintetizator Roland TB-303. Este ingredientul secret din sunetul acid bass original. Acele clasice riff-uri de bas scartaite produse de TB-303 pot fi intalnite de la inceputurile Chicago House pana mai aproape de zilele noastre la artisti ca Plastikman, Squarepusher si Aphex Twin.

Este interesant de stiut ca Roland nu a a gandit TB-303 pentru a fi utilizat in muzica dance. A fost creat initial ca un ajutor pentru chitaristii care exerseaza. Ei si-au imaginat ca utilizatorii vor programa sintetizatorul sa redea linia de bas si vor improviza alaturi de el. Din nefericie, au existat o serie de probleme: erau cam complicat de programat, nu sunau foarte bine ca inlocuitor de chitara bas si erau destul de scumpe. Ca sa-si limiteze pierderile, Roland a decis sa opreasca productia duoa ce a vandut 10000 de bucati si dupa ce au zacut cativa ani nefolosite prin dulapurile chitaristilor au inceput sa apara in vitrinele magazinelor second hand. Aceste TB-303 abandonat asteptau sa fie descoperite de o noua generatie de inovatori care au inceput sa le foloseasca in moduri la care Roland nu se gandise si sa creeze sunete noi incitante. Asa s-a nascut Acid House.

Daca sa pui mana pe un TB-303 nu e prea usor, te vei bucura sa afil ca poti sa iti transformi Raspberry Pi intr-un astfel de sintetizatori folosind puterea Sonic Pi. Pornest Sonic Pi si copiaza codul asta intr-un buffer gol, apoi apasa Run:

```
use_synth :tb303
play :e1
```
    
Am obtinut pe loc acid bass. Sa ne jucam putin...

## Sa scartaie basul

Mai intai, sa construim un generator live de arpegii pentru a face lucrurile mai distractive. In tutorialul precedent am vazut cum riff-urile pot fi doar liste de note care se repeta una cate una, luand-o de la capat cand ajungem la sfarsit. Sa cream o bucla live care face exact acest lucru:

```
use_synth :tb303
live_loop :squelch do
  n = (ring :e1, :e2, :e3).tick
  play n, release: 0.125, cutoff: 100, res: 0.8, wave: 0
  sleep 0.125
end
```

Sa privim fiecare linie.

1. Pe prima linie alegem ca sintetizatorul implicit sa fie 'tb303', folosind functia 'use_synth'.
2. Pe linia a doua cream o bucla live numita ':squelch' care se va relua iar si iar.
3. Linia a treia este locul unde cream riff-ul - o lista de note (E in octavele 1, 2 si 3) pe care o parcurgem ciclic folosind '.tick'. Definim 'n' pentru a reprezenta nota curent din riff. Semnul egal inseamna ca atribuim valoarea din drepata numelui din stanga sa. Aceasta va fi diferita la fiecare trecere. Prima data, 'n' va avea valoarea ':e1'. A doua oara va fi ':e2;, urmata de ':e3' si apoi iar ':e1', reluandu-se la infinit.
4. Linia patru este locul in care pornim cu adevarat ':tb303' Sunt folositi cativa parametri interesanti aici: 'release:', 'cutoff:, 'res:' si 'wave:', pe care ii discutam in continuare.
5. Linia 5 este cea in care cerem pauza ('sleep') - cerem buclei sa se reia la fiecare '0.125'secunde sau de 8 ori pe secunda la viteza implicita de 60 BPM.
6. Linia 6 este sfarsitul ('end') buclei live. Ea spune Sonic Pi unde se termina partea care se repeta in bucla.

Cat timp incerci sa intelegi ce se intampla, scrie codul de mai sus si apasa butonul Run. Ar trebui sa auzi cum ':tb303' intra in actiune. Aici este toata distractia, sa incepem sa programam live!

In timp ce se executa bucla, schimba valoarea pentru 'cutoff:' la '110'. Acum apasa pe Run din nou. Ar trebui sa auzi cum sunetul devine putin mai dur si mai scartait. Scrie '120' si apasa pe Run. Apoi '130'. Asculta cum o valoare mai mare pentru cutoff face sunetul mai intens si mai patrunzator. Coboara valoarea la '80' cand simti ca e nevoie de o pauza. Apoi repeta de cate ori doresti. Nu te teme, voi fi tot aici...

Un alt parametru cu care te poti juca este'res:'. Acesta controleaza nivelul de rezonanta al filtrului. O rezonanta inalta este o caracteristica a sunetelor acid bass. Acum avem valoarea pentru 'res:' setata la '0.8'. Incearca sa o maresti putin la '0.85', apoi la '0.9' si in final la 0.95'. Vei constata ca o valoare pentru cutoff gen '110' sau mai ridicata va face diferentele mai usor de auzit. Fa o nebunie si introdu '0.999' pentru niste sunete traznite. La o valoare atat de ridicata pentru 'res' vei auzi cum filtrul cutoff rezoneaza atata de mult incat incepe sa scoata propriile sunete!

In fine, un impact puternic asupra timbtului il are schimbarea valorii pentru 'wave:' in '1'. Aceasta este alegerea oscilatorului sursa. Valoarea '0' este pentru unda dinte de fierastrau, '1' este pentru impulsuri rectangulare, iar '2' pentru o forma de unda triunghiulara.

Desigur, poti incerca diferite riff-uri schimband notele din lista sau chiar alegand notele din game sau acorduri. Distreaza-te cu primul tau sintetizator acid bass.

## Disectia TB-303

Designul TB-303 original este de fapt destul de simplu. Dupa cum vezi in diagrama urmatoare sunt doar 4 componente de baza.

![Designul TB-303](../../../etc/doc/images/tutorial/articles/A.05-acid-bass/tb303-design.png)

Prima este unda produsa de oscilator - materia prima pentru sunet. In cazul acesta avem o unda rectangulara. Apoi este anvelopa de amplitudine a oscilatorului care controleaza amplitudineea undei rectangulare in timp. Acestea sunt accesibile in Sonic Pi folosind 'attack:', 'decay:', 'sustain:' si 'release:' impreuna cu nivelele lor. Pentru mai multe informatii citeste sectiunea 2.4 'Anvelopa de timp' din tutorialul inclus in program. Trecem apoi unda rectangulara careia i-a fost aplicata anvelopa printr-un filtru rezonant 'trece jos' (low pass). Acesta taie frecventele inalte si are si un efect placut de rezonanta. Aici incepe partea interesanta. Valoarea de cutoff pentru acest filtrul are propria sa anvelopa. Aste ne ofera un control incredibil asupra timbrului sunetului prin modificarea ambelor anvelope. Sa aruncam o privire:

```
use_synth :tb303
with_fx :reverb, room: 1 do
  live_loop :space_scanner do
    play :e1, cutoff: 100, release: 7, attack: 1, cutoff_attack: 4, cutoff_release: 4
    sleep 8
  end
end
```
    
Pentru fiecare parametru al anvelopei standard exista un echivalent pentru 'cutoff_' in cazul sintetizatorului ':tb303'. Deci, pentru a shimba timpul de atac pentru cuttof putem folosi parametrul 'cutoff_attack'. Copiaza codul de mai sus intr-un buffer gol si apasa Run. Vei auzi un sunet ca un ciripit. Incearca sa schimbi timpul pentru 'cutoff_attack' in '1' si apoi in '0.5'. Acum incearca '8'.

Observi ca am trecut totul printr-un efect ':reverb' pentru atmosfera. Incearca alte efecte pentru a vedea ce se intampla!

## Sa punem totul cap la cap

La final iata o piesa pe care am compus-o folosind ideile prezentate in acest tutorial. Copiaz-o intr-un buffer gol, asculta pentru un timp apoi incepe sa programezi live aducand propriile modificari. Vezi ce sunete interesante poti produce din ea! Ne vedem data viitoare...

```
use_synth :tb303
use_debug false
 
with_fx :reverb, room: 0.8 do
  live_loop :space_scanner do
    with_fx :slicer, phase: 0.25, amp: 1.5 do
      co = (line 70, 130, steps: 8).tick
      play :e1, cutoff: co, release: 7, attack: 1, cutoff_attack: 4, cutoff_release: 4
      sleep 8
    end
  end
 
  live_loop :squelch do
    use_random_seed 3000
    16.times do
      n = (ring :e1, :e2, :e3).tick
      play n, release: 0.125, cutoff: rrand(70, 130), res: 0.9, wave: 1, amp: 0.8
      sleep 0.125
    end
  end
end
```
