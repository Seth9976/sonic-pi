A.8 Cum sa devii un VJ Minecraft

# Cum sa devii un VJ Minecraft

![Imaginea 0](../../../etc/doc/images/tutorial/articles/A.08-minecraft-vj/minecraft-vj-0-small.png)


Minecraft Pi


Toata lumea a jucat Minecraft. Toti ati construit structuri impresionante, ati gandit capcane iscusite si probabil ati creat trasee pentru minecart controlate de comutatoare si circuite din redstone. Cati dintre voi ati interpretat muzica in Minecraft. Pariez ca nu stiati ca puteti folosi Minecraft ca sa creati efecte vizuale impresionante, la fel ca un VJ profesionist.

Daca singurul mod in care ai schimbat ceva in Minecraft a fost cu ajutorul mouse-ului, probabil te-ai chinuit destul de mult sa schimbi lucrurile suficient de rapid. Din fericire pentru tine Raspberry Pi iti vine in ajutor cu o versiune de Minecraft care poate fi controlata prin intermediul programarii. Vine de asemenea cu o aplicatie numita Sonic Pi care face programarea in Minecraft nu doar foarte facila ci si incredibil de distractiva.

In articolul de azi iti voi arata cateva trucuri pe care le-am folosit pentru interpretarile din cluburile de noapte sau scene muzicale din toata lumea.

Sa incepem...

## Inceputul

Sa incepem cu un exercitiu usor de incalzire pentru a ne reaminti lucrurile de baza. Pentru inceput, porneste Raspberry Pi si apoi lanseaza atat Minecraft cat si Sonic Pi. In Minecraft, creeaza o lume noua, iar in Sonic Pi alege un buffer gol si scrie codul de mai jos:

```
mc_message "Sa incepem..."
```
    
Apasa butonul Run si vei vedea un mesaj in fereastra Minecraft. OK, suntem gata sa incepem, hai sa ne distram...

## Furtuna de nisip

Cand folosim Minecraft pentru a crea efecte vizuale incercam sa ne gandim ce va arata interesant si va fi si usor de generat prin cod. Un truc haios este sa creezi o furtuna de nisip lasand sa cada blocuri de nisip din cer. Pentru asta avem nevoie doar de cateva functii de baza:

* 'sleep' - pentru a insera o pauza intre actiuni
* 'mc_location' - pentru a afla pozitia noastra curenta
* 'mc_set_block' - pentru a plasa blocuri de nisip la o anumita pozitie
* 'rrand' - pentru a genera valori aleatoare intr-un anumit interval
* 'live_loop' - pentru a face ca ploaia de nisip sa cada in continuu

Daca nu esti familiarizat cu vreuna dintre functiile de mai sus, cum ar fi 'rrand', scrie numele ei intr-un buffer, fa click pe el si apoi apasa combinatia de taste 'Control-i' pentru a afisa informatiile despre functie din documentatia interna. O alta varianta ar fi sa mergi la panoul *lang* din sistemul de ajutor si sa cauti direct functia alaturi de alte chestii incitante pe care le poti face.

Sa facem sa ploua putin inainte sa se dezlantuie furtuna cu toata forta. Vezi care este pozitia curenta si folosteste-o ca sa creezi cateva blocuri de nisip sus pe cer, in apropiere:

```
x, y, z = mc_location
mc_set_block :sand, x, y + 20, z + 5
sleep 2
mc_set_block :sand, x, y + 20, z + 6
sleep 2
mc_set_block :sand, x, y + 20, z + 7
sleep 2
mc_set_block :sand, x, y + 20, z + 8
```
    
Cand apesi pe Run va trebui sa te uiti putin in jurul tau deoarece blocurile ar putea incepe sa cada in spatele tau, in functie de directia in care te uitai. Nu te ingrijora daca le-ai ratat, apasa Run din nou pentru inca un lot de ploaie de nisip - doar asigura-te ca te uiti in directia potrivita!

Sa trecem rapid in revista ce se intampla aici. Pe prima linia am citit pozitia lui Steve sub forma de coordonate, folosind functia 'mc_location', si le-am pus in variabilele 'x', 'y' si 'z'. Apoi, pe liniile urmatoare am folosit 'mc_set_block' pentru a crea niste blocuri de nisip la aceleasi coordonate cu ale lui Steve, dar cu niste modificari. Alegem aceeasi valoare pentru x, o valoare pentru y mai mare cu 20 (adica mai sus) si apoi valori care cresc succesiv pentru z astfel incat nisipul sa cada pe o linie care se indeparteaza de Steve.

Ce-ar fi sa te joci putin modificand codul? Incearca sa adaugi mai multe linii, sa schimbi valoarea pentru 'sleep', sa amesteci ':sand' cu ':gravel' si sa alegi coordonate diferite. Experimenteaza si distreaza-te!

## Puterea buclelor live

OK, e timpul sa pornim furtuna folosind forta 'live_loop' - bagheta magica a Sonic Pi care permite folosirea la maxim a puterii programarii live prin modificarea codului in timp ce ruleaza!

```
live_loop :sand_storm do
  x, y, z = mc_location
  xd = rrand(-10, 10)
  zd = rrand(-10, 10)
  co = rrand(70, 130)
  synth :cnoise, attack: 0, release: 0.125, cutoff: co
  mc_set_block :sand, x + xd, y+20, z+zd
  sleep 0.125
end
```
    
Ce distractiv! Parcurgem bucla rapid (de 8 ori pe secunda) si la fiecare trecere citim pozitia lui Steve la fel ca mai inainte, iar apoi generam 3 valori aleatoare:

* 'xd' - diferenta pe axa x care va fi intre -10 si 10
* 'zd' - diferenta pe axa z, de asemenea intre -10 si 10
* 'co' - valoarea de cutoff pentru filtrul trece jos, intre 70 si 130

Folosim apoi aceste valori in functiile 'synth' si 'mc_set_block' obtinand nisipul care cade in pozitii aleatoare in jurul lui Steve impreuna cu un sunet asemanator cu cel al ploii generat de sintetizatorul ':cnoise'.

Daca nu stii inca ce sunt buclele live - ele sunt cele care dau tot farmecul cand folosim Sonic Pi. In timp ce codul ruleaza si nisipul cade, incearca sa schimbi una dintre valori, de exemplu valoare pentru pauza la '0.25' sau tipul blocurilor din ':sand' in ':gravel'. Acum apasa run *din nou*. Super! Lucrurile s-au schimbat fara a opri programul. Asta este modalitatea prin care poti sa te manifesti ca un VJ adevarat. Continua sa exersezi si sa schimbi parametrii. Cat de mult poti schimba efectele vizuale fara a opri executia?

## Modele de blocuri spectaculoase

![Imaginea 1](../../../etc/doc/images/tutorial/articles/A.08-minecraft-vj/minecraft-vj-1-small.png)

O alta metoda de a genera imagini interesante este sa generezi pereti imensi cu model care se deplaseaza. Pentru acest efect trebuie sa trecem de la plasarea de blocuri aleator la plasarea lor intr-o ordine prestabilita. Putem face asta creand o iteratie in iteratie (apasa pe butonul Help si mergi la sectiunea 5.2 a tutorialului, "Iteratii si bucle" pentru mai multe informatii despre iteratii). Simpaticul '|xd|' de dupa do inseamna ca 'xd' va fi setat pentru fiecare valoare a iteratiei. Deci prima fata va fi 0, apoi 1, apoi 2 ... etc. Introducand o iteratie intr-o alta ca aici putem genera toate coordonatele dintr-un patrat. Putem apoi alege aleator tipurile de blocuri dintr-o lista circulara pentru un efect interesant:

```
x, y, z = mc_location
bs = (ring :gold, :diamond, :glass)
10.times do |xd|
  10.times do |yd|
    mc_set_block bs.choose, x + xd, y + yd, z
  end
end
```

Simplu dar elegant. Cat ne distram aici, incearca sa schimbi  `bs.choose` in `bs.tick` pentru a trece de la un model aleator la unul mai regulat. Incearca sa schimbi tipurile blocurilor si daca esti mai indraznet ai putea pune asta intr-un 'live_loop' astfel incat modelul sa se schimbe automat.

Acum, pentru trucul tau special de VJ, schimba cele 2 `10.times` in `100.times` si apasa Run. Kaboom! Un perete gigant de caramizi plasate aleator. Imagineaza-ti cat timp ti-ar lua sa il construiesti manual cu mouse-ul! Apasa de doua ori rapid pe spatiu pentru a activa modul de zbor si incepe sa te plimbi pentru ca efectul vizual sa fie si mai interesant. Nu te opri aici, foloseste-ti imaginatia si incearca sa iti aplici ideile folosind puterea programarii in Sonic Pi. Cand consideri ca ai exersat suficient, redu lumina si pune in scena un show pentru prietenii tai!
