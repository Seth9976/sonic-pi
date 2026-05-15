A.6 Minecraft muzical

# Minecraft muzical


Minecraft Pi


Salut, bine ati revenit! In precedentele tutoriale ne-am concentrat doar pe functionalitatile muzicale ale Sonic Pi (transformarea Raspberry Pi intr-un instrument muzical gata pentru interpretare). Pana acum am invatat cum sa:

* Programam live - schimband sunetele din mers,
* Programam niste beat-uri geniale,
* Generam melodii cool folosind sintetizatoarele,
* Re-cream vestitul sunet acid-bass al sintetizatorului TB-303.

Am insa mult mai multe sa va arat (si voi continua sa fac asta in numerele viitoare). Luna asta, insa, am sa va arat ceva ce Sonic Pi stie sa faca dar probabil nici nu va ganditi: sa controleze Minecraft.

## Salut lume Minecraft!

OK, sa incepem. Porneste Rasberry Pi, lanseaza Minecraft Pi si creeaza o lume noua. Acum porneste Sonic Pi si redimensioneaza si aseaza ferestrele astfel incat sa vezi atat Sonic Pi cat si Minecraft Pi in acelasi timp.

Intr-un buffer nou scrie asta:

```
mc_message "Hello Minecraft from Sonic Pi!"
```
    
Acum, apasa Run. Boom! Mesajul tau a aparut in Minecraft! Cat de greu a fost? Acum, opreste-te putin din citit acest tutorial si joaca-te putin cu propriile mesaje. Distreaza-te!

![Imaginea 0](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-0-small.png)

## Teleportare sonica

Acum sa exploram putin. Varianta obisnuita este sa iei mouse-ul si tastatura si sa incepi sa te plimbi in jur. Merge, dar este incet si plictisittor. Ar fi mult mai bine daca ai avea o masina care ar putea sa te teleporteze. Ei bine, Sonic Pi are una. Incearca asta:

```
mc_teleport 80, 40, 100
```
    
Genial! Ai ajuns sus, departe. Daca nu aveai activat modul de zbor ai cazut inapoi pana pe pamant. Daca apeste de doua ori rapid pe spatiu ca asa comuti in modul de zbor si te teleportezi din nou, vei incepe sa plutesti din pozitia in care ai aparut.

Acum sa vedem ce inseamna acele numere. Avem trei numere care descriu coordonatele locului din lume in care ne aflam. Fiecare numar are un nume - x, y si z:

* x - cat de departe la stanga sau la dreapta (80 in exemplul nostru)
* y - cat de sus vrem sa ajungem (40 in exemplul nostru)
* z - cat de departe inainte sau inapoi (100 in exemplul nostru)

Alegand diferite valori pentru x, y sau z, putem sa ne teleportam *oriunde* in lumea noastra. Incearca! Alege numere diferite si vezi unde ajungi. Daca ecranul devine negru inseamna ca te-ai teleportat sub pamant sau intr-un munte. Alege o valoare mai mare pentru y si vei iesi din nou deasupra solului. Continua sa explorezi pana gasesti un loc care iti place...

Folosind ideile de pana acum, sa construim o masina de teleportat care scoate un sunet haios cand te deplaseaza in lumea Minecraft:

```
mc_message "Preparing to teleport...."
sample :ambi_lunar_land, rate: -1
sleep 1
mc_message "3"
sleep 1
mc_message "2"
sleep 1
mc_message "1"
sleep 1
mc_teleport 90, 20, 10
mc_message "Whoooosh!"
```
    
![Imaginea 1](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-1-small.png)

## Blocuri magice

Ai gasit un loc interesant, acum sa incepem sa construim. Ai putea face ca pana acum sa incepi sa faci click in disperare cu mouse-ul pentru a plasa blocuri la pozitia cursorului. Sau ai putea sa folosesti puterile magice ale Sonic Pi. Incearca asta:

```
x, y, z = mc_location
mc_set_block :melon, x, y + 5, z
```

Acum priveste in sus! E un pepene pe cer! Priveste o clipa la cod. Ce am facut? Pe prima linia am aflat pozitia curenta a lui Steve in variabilele x, y si z. Acestea corespund coordonatelor descrise anterior. Folosim aceste coordonate in functia 'mc_set_block' care plaseaza blocul pe care l-ai ales la coordonatele specificate. Pentru a construi ceva mai sus in cer trebuie sa marim valoarea pentru y, motiv pentru care am adaugat 5 la ea. Sa construim o poteca:

```
live_loop :melon_trail do
  x, y, z = mc_location
  mc_set_block :melon, x, y-1, z
  sleep 0.125
end
```

Acum, mergi in fereastra Minecraft, asigura-te ca esti in modul de zbor (apasa de doua ori pe bara de spatiu in caz contrar) si zboara la intamplare. Priveste in urma ta si vei vedea o poteca formata din pepeni. Incearca sa creezi modele cat mai introtocheate pe cer.

## Programare live in Minecraft

Cei care au urmarit acest tutorial pe parcursul ultimelor lui sunt probabil dati pe spate. Poteca din pepeni este interesanta, dar cel mai surprinzator este ca poti folosi 'live_loop' cu Minecraft! Pentru cei care nu stiu, 'live_loop' este o putere magica a Sonic Pi pe care niciun alt limbaj de programare nu o are. Iti permite sa rulezi mai multe bucle in acelasi timp si te lasa sa le schimbi in timp ce ruleaza. Sunt incredibil de puternice si foarte distractive. Eu folosesc 'live_loop' pentru a canta in cluburi cu Sonic Pi. DJ-ii folosesc discuri, eu folosesc bucle live :) Dar astazi vom programa live si muzica si Minecraft.

Sa incepem. Ruleaza codul de mai sus si incepe sa construiesti poteca din pepeni din nou. Acum, fara sa opresti codul, schimba ':melon' cu ':brick' si apasa pe Run. Acum lasi in urma ta o poteca din caramizi. Cat de simplu a fost! Ti-ar placea sa ai si niste muzica in timp ce faci asta? E usor. Incearca:

```
live_loop :bass_trail do
  tick
  x, y, z = mc_location
  b = (ring :melon, :brick, :glass).look
  mc_set_block b, x, y -1, z
  note = (ring :e1, :e2, :e3).look
  use_synth :tb303
  play note, release: 0.1, cutoff: 70
  sleep 0.125
end
```
    
Acum, in timp ce ruleaza, incepe sa schimbi codul. Schimba tipul de blocuri - incearca ':water', ':grass' sau blocul tau favorit. In acelasi timp incearca sa schimbi valoarea pentru cutoff din '70' in '80 si apoi in '100' Nu-i asa ca este distractiv?

## Sa punem totul cap la cap

![Imaginea 2](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-2-small.png)

Sa punem cap la cap ce am vazut pana acum si sa mai adaugam putina magie. Sa combinam abilitatea de a ne teleporta cu plasarea de blocuri si cu muzica pentru a realiza un videoclip Minecraft. Nu te ingrijora daca nu intelegi totul, doar scrie codul asta si joaca-te cu el schimband niste valori in timp ce este redat. Distreaza-te! Ne vedem data viitoare...
    
```
live_loop :note_blocks do
  mc_message "This is Sonic Minecraft"
  with_fx :reverb do
    with_fx :echo, phase: 0.125, reps: 32 do
      tick
      x = (range 30, 90, step: 0.1).look
      y = 20
      z = -10
      mc_teleport x, y, z
      ns = (scale :e3, :minor_pentatonic)
      n = ns.shuffle.choose
      bs = (knit :glass, 3, :sand, 1)
      b = bs.look
      synth :beep, note: n, release: 0.1
      mc_set_block b, x+20, n-60+y, z+10
      mc_set_block b, x+20, n-60+y, z-10
      sleep 0.25
    end
  end
end
live_loop :beats do
  sample :bd_haus, cutoff: 100
  sleep 0.5
end
```
