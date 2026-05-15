A.2 Programarea live

# Programare live

Fasciculele laser taiau norii grosi de fum in timp ce subwooferul pompa injecta un bas puternic in multime. Atmosfera era incalzita de un mix de sintetizatoare si de dansatori. Totusi ceva nu era chiar in regula in club. Proiectat in culori stralucitoare deasupra cabinei DJ-ului era un text care se schimba, clipea si dansa. Nu erau niste efecte vizuale traznite, ci proiectia ecranului Sonic Pi ruland pe un Raspberry Pi. Ocupantul cabinei DJ-ului nu invartea discuri ci scria, modifica si evalua codul. In direct. Asta e Programarea Live.

![Sam Aaron Programand live](../../../etc/doc/images/tutorial/articles/A.02-live-coding/sam-aaron-live-coding.png)

Poate parea o poveste trasa de par dintr-un club din viitor, dar crearea muzicii prin cod ca mai sus este pe un trend crescator si este adesea descrisa ca Programare Live (http://toplap.org). Una dintre directiile spre care se indreapta in prezent acest mode de a face muzica este Algorave (http://algorave.com) - spectacole in care artisti ca mine programeaza muzica pe care lumea danseaza. Totusi, nu trebuie sa fii neaparat intr-un club ca sa programezi live - cu Sonic Pi poti sa faci asta oriunde poti sa iei cu tine un Raspberry Pi si o pereche de casti sau niste difuzoare. Cand vei fi ajuns la sfarsitul acestui articol, vei fi in stare sa programezi propriile ritmuri si sa le modifici pe viu. Ce vei face dupa aceea depinde doar de imaginatia ta.

## Bucla live

Cheia programarii live este sa stapanesti 'live_loop'. Sa aruncam o privire:

```
live_loop :beats do
  sample :bd_haus
  sleep 0.5
end
```

Exista 4 elemente de baza intr-un 'live_loop'. Primul este numele. Bucla noastra de mai sus se numeste ':beats'. Poti sa denumesti bucla cum vrei. Lasa-te purtat de imaginatie, fii creativ. Eu folosesc adesea nume care transmit ceva audientei despre muzica pe care o produc. Al doilea element este cuvantul cheie 'do' care marcheaza inceputul buclei. Al treilea este cuvantul cheie 'end' care marcheaza sfarsitul buclei si, in fine, exista continutul care arata ceea ce se va repeta in bucla respectiva - partea dintre 'do' so 'end'. In acest caz am redat in mod repetat un esantion de toba bas si am asteptat jumatate de bataie. Asta produce  un bas regulat. Copiaza textul intr-un buffer Sonic Pi si apasa Run. Boom, Boom, Boom!

## Redefinirea din mers

Ok, deci ce e asa special la o bucla 'live_loop'? Pana acum pare sa fie doar o bucla obisnuita cu un nume mai pompos. Ei bine, frumusetea buclelor de tip 'live_loop' este ca pot fi redefinite din mers. Asta inseamna ca poti schimba ce fac in timp ce ele se executa. Acesta este secretul programarii live cu Sonic Pi. Sa ne jucam putin:

```
live_loop :choral_drone do
  sample :ambi_choir, rate: 0.4
  sleep 1
end
```

Acum apasa butonul Run sau combinatia 'M-r'. Poti asculat niste sunete corale minunate. In timp ce canta, schimba viteza de la '0.4' la '0.38'. Apasa din nou Run. Uau! Ai auzit cum s-a schimbat tonalitatea corului? Schimba la loc cu '0.4' ca sa fie ca la inceput. Acum, coboara la '0.2', apoi la '0.19' si revino la '0.4'. Observi ca schimbarea din mers a unui singur parametru poate sa iti ofere un control real asupra muzicii. Acum joca-te cu viteza de redare - alege valorile pe care le doresti. Incearca numere negative, numere foarte mici sau numere mari. Savureaza rezultatul!

## Pauzele sunt importante

Una dintre cele mai importante lectii despre 'live_loop' este ca are nevoie de pauze. Sa luam urmatorul 'live_loop':

```
live_loop :infinite_impossibilities do
  sample :ambi_choir
end
```

Daca incerci sa rulezi acest cod, vei observa imediat ca Sonic Pi se plange ca 'live_loop' nu a facut nicio pazua (sleep). Acesta este sistemul de siguranta care s-a activat! Sa ne gandim putin ce cere codul respectiv computerului. Asa e, ii cere computerului sa redea la nesfarsit esantioane corale de lungime zero. Fara sistemul de siguranta bietul computer ar incerca sa redea o infinitate de esantioane si ar lua-o razna. Deci, tine minte, orice 'live_loop' trebuie sa contina un 'sleep'.


## Combinarea sunetelor

Muzica inseamna multe lucruri care se intampla in acelasi timp. Tobele in acelasi timp cu basul, in acelasi timp cu vocea, in acelasi timp cu chitarele... In lumea computerelor acestea se numesc procese concurente iar Sonic Pi ne ofera o cale uimitor de simpla pentru a face mai multe lucruri in acelasi timp. Pur si simplu folosim mai multe 'live_loop' in acelasi timp!

```
live_loop :beats do
  sample :bd_tek
  with_fx :echo, phase: 0.125, mix: 0.4 do
    sample  :drum_cymbal_soft, sustain: 0, release: 0.1
    sleep 0.5
  end
end
live_loop :bass do
  use_synth :tb303
  synth :tb303, note: :e1, release: 4, cutoff: 120, cutoff_attack: 1
  sleep 4
end
```

Aici avem doua bucle 'live_loop', una rapida care genereaza batai de taler si alta mai lenta cu un sunet dement de bas.

Interesant cu buclele 'live_loop' multiple este ca fiecare isi gestioneaza separat timpul. Asta insemana ca este foarte usoe sa creezi structuri poliritmice interesante si chiar sa te joci cu fazele in stilul Steve Reich. Asculta asta:

```
# Steve Reich's Piano Phase
notes = (ring :E4, :Fs4, :B4, :Cs5, :D5, :Fs4, :E4, :Cs5, :B4, :Fs4, :D5, :Cs5)
live_loop :slow do
  play notes.tick, release: 0.1
  sleep 0.3
end
live_loop :faster do
  play notes.tick, release: 0.1
  sleep 0.295
end
```

## Sa punem totul cap la cap

In fiecare dintre aceste tutoriale incheiem cu un exemplu final sub forma unei noi piese muzicale care se bazeaza pe ideile prezentate. Citeste codul acesta si incearca sa-ti imaginezi ce face. Apoi, copiaza-l intr-un buffer nou in Sonic Pi si apasa Run ca sa vezi cum suna de fapt. Schimba apoi unul dintre numere sau transforma o parte din cod in comentariu. Vezi daca poti folosi rezultatul ca un punct de plecare pentru o noua piesa si, in primul rand, simte muzica! Ne vedem data viitoare...

```
with_fx :reverb, room: 1 do
  live_loop :time do
    synth :prophet, release: 8, note: :e1, cutoff: 90, amp: 3
    sleep 8
  end
end
live_loop :machine do
  sample :loop_garzul, rate: 0.5, finish: 0.25
  sample :loop_industrial, beat_stretch: 4, amp: 1
  sleep 4
end
live_loop :kik do
  sample :bd_haus, amp: 2
  sleep 0.5
end
with_fx :echo do
  live_loop :vortex do
    # use_random_seed 800
    notes = (scale :e3, :minor_pentatonic, num_octaves: 3)
    16.times do
      play notes.choose, release: 0.1, amp: 1.5
      sleep 0.125
    end
  end
end
```
