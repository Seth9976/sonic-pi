A.9 Randomizarea

# Navigand pe curenti aleatori

In episodul 4 al acestui serial de tutoriale am aruncat o privire asupra randomizarii in timp ce am programat cateva riff-uri incitante. Tinand cont ca randomizarea este o parte atat de importanta a interpretarilor mele live, m-am gandit ca ar fi util sa vorbim mai in detaliu despre elementele ei de baza. Deci, ia-ti amuleta norocoasa si sa navigam pe curenti aleatori!

## Nu exista aleator

Primul lucru pe care trebuie sa-l inveti si care s-ar putea sa ti se para surprinzator cand te joci cu functiile de randomizare este ca de fapt nu sunt chiar aleatoare. Ce inseamna asta? Ei bine, sa facem niste teste. Mai intai, gandeste-te la un numar intre 0 si 1. Tine-l minte fara sa mi-l spui. Acum, sa incerc sa-l ghicesc... era '0.321567'? Nu? Bleah, e evident ca nu-s bun la jocul asta. Hai sa mai incerc o data, dar sa lasam Sonic Pi sa aleaga numarul de data asta. Porneste Sonic Pi v2.7+ si cere-i un numar aleator, din nou fara sa mi-l spui mie:

```
print rand
```

Acum sa vedem... era `0.75006103515625`? Da! Ha, ha, vad ca esti putin neincrezator. Poate a fost doar chestie de noroc. Hai sa incercam din nou. Apasa butonul Run si vezi ce primesti... Ce? Din nou `0.75006103515625`? E clar ca nu este aleator! Ai dreptate, nu este.

Ce se intampla aici? Cuvantul elevat folosit in lumea calculatoarelor pentru asta este determinism. Asta inseamna ca nimic nu este la intamplare ci totul este bazat pe predestinare. Versiunea ta de Sonic Pi este predestinata sa returneze mereu `0.75006103515625` in programul de mai sus. Asta pare sa fie o chestie inutila, dar crede-ma ca este una dintre partile cele mai tari din Sonic Pi. Daca te tii de ea vei invata cum sa te bazezi pe natura determinista a randomizarii in Sonic Pi ca bloc de baza pentru constructia compozitiilor tale sau a interpretarii live ca DJ.

## O melodie aleatoare

Cand Sonic Pi porneste, incarca in memorie o secventa de 441 000 valori aleatoare pre-calculate. Cand apelezi o functie pentru numere aleatoare, cum ar fi 'rand' sau 'rrand', acest sir aleator este folosit pentru a genera rezultatul asteptat. Fiecare apel al unei functii pentru numere aleatoare consuma o valoare din sir. Deci al zecelea apel al unei functii pentru numere aleatoare va folosi a zecea valoare din sir. De asemenea, de fiecare data cand apesi pe butonul Run, sirul este resteat pentru acea executie. Acesta este motivul pentru care am reusit sa prezic rezultatul functiei 'rand', iar melodia 'random' a sunat mereu la fel. Versiunea Sonic Pi a fiecaruia dintre voi foloseste exact acelasi sir de valori aleatoare, ceea ce este foarte important cand impartasesti creatiile tale cu altii.

Sa ne folosim de ce am aflat ca sa generam o melodie aleatoare repetabila:

```
8.times do
 play rrand_i(50, 95)
 sleep 0.125
end
```

Copiaza codul asta intr-un buffer gol si apasa Run. Vei auzi o melodie constand in note 'aleatoare' intre 50 si 95. Cand se termina, apasa din nou pe Run ca sa auzi exact aceeasi melodie din nou.

## Functii utile pentru randomizare

Sonic Pi contine o serie de functii utile pentru a lucra cu sirul aleator. Iata o lista cu cateva dintre cele mai folositoare:

* 'rand' - returneaza urmatoarea valoare din sirul aleator
* 'rrand' - returneaza o valoare aleatoare intr-un interval
* 'rrand_i' - returneaza un numar intreg intr-un interval
* 'one_in' - returneaza 'true' sau 'false' cu o probabilitate specificata
* 'dice' - imita datul cu zarul si returneaza o valoare intre 1 si 6
* 'choose' - alege o valoare aleatoare dintr-o lista

Verifica documentatia pentru aceste functii in sistemul de ajutor pentru informatii detaliate si exemple.

## Resetarea sirului aleator

Chiar daca abilitatea de a repeta o secventa de note este esentiala pentru a-ti permite sa redai din nou un riff pe ringul de dans, s-ar putea sa nu fie exact riff-ul pe care ti l-ai dorit. N-ar fi interesant daca am putea incerca mai multe riff-uri si sa-l alegem pe cel care a sunat cel mai bine? Aici incepe magia.

Putem seta manual sirul folosind functia 'use_random_seed'. In lumea computerelor, seed este valoarea de initializare a sirului aleator sau punctul de pornire din care poate porni un nou sir de valori aleatoare. Sa incercam:

```
use_random_seed 0
3.times do
  play rrand_i(50, 95)
  sleep 0.125
end
```

Super, avem primele trei note ale melodiei noastre aleatoare de mai sus: '84', '83' si '71'. Am putea insa schimba seed-ul cu altceva. Sa incercam asta:

```
use_random_seed 1
3.times do
  play rrand_i(50, 95)
  sleep 0.125
end
```

Interesant, am obtinut '83', '71' si '61'. Probabil ai observat ca primele doua numere sunt aceleasi ca ultimele doua numere de data trecuta - nu este o coincidenta.

Iti aduci aminte ca sirul aleator este doar o lista gigantica de valori 'pre-generate'. Folosirea unui seed inseamna pur si simplu pozitionarea in aceasta lista. Un alt mod de a vedea acest lucru este sa iti imaginezi un pachet enorm de carti amestecate. Folosirea unui seed este la fel cu a taia pachetul de carti intr-o anumita pozitie. Partea cea mai frumoasa este ca tocmai aceasta abilitate de a te pozitiona precis in sirul aletor ne da o mare putere cand cream muzica.

Sa revenim la melodia noastra aleatoare din 8 note inarmati cu aceasta abilitate de a reseta sirul aleator, dar hai sa adaugam si o bucla live ca sa putem experimenta in timp ce canta muzica:

```
live_loop :random_riff do    
  use_random_seed 0
  8.times do
    play rrand_i(50, 95), release: 0.1
    sleep 0.125
  end
end
```
  
Acum, in timp ce canta, schimba valoarea pentru seed din '0' in altceva. Incearca '100', sau '999'. Incearca propriile valori, experimenteaza si joaca-te. Vezi ce seed genereaza riff-ul care iti place cel mai mult.

## Sa punem totul cap la cap

Tutorialul de luna asta a fost o incursiune tehnica in modul de functionare a randomizarii in Sonic Pi. Sper ca ti-ai putut crea o idee despre cum functioneaza si cum te poti folosi de randomizare intr-un mod fiabil pentru a crea secvente repetabile in muzica ta. E important sa evidentiem ca poti folosi randomizarea repetabile *oriunde* doresti. De exemplu, poti randomiza amplitudinea notelor, tempoul pentru ritm, valoarea pentru reverb, sintetizatorul curent, mixul pentru un efect, etc etc. Intr-un episod viitor vom privi mai indeaproape cateva dintre aceste utilizari, dar deocamdata iti voi prezenta un scurt exemplu.

Scrie codul de mai jos intr-un buffer gol, apasa Run, apoi incepe si schimba seed-ul, apasa din nou Run (cat timp inca ruleaza) si incearca diferitele sunete, ritmuri si melodii pe care le poti obtine. Cand gasesti ceva dragut, tine minte seed-ul pentru a putea reveni la el. Dupa ce ai gasit mai multe seed-uri care iti plac, poti sa realizezi o interpretare live pentru prietenii tai schimband intre diferitele valori pentru a crea o piesa completa.

```
live_loop :random_riff do
  use_random_seed 10300
  use_synth :prophet
  s = [0.125, 0.25, 0.5].choose
  8.times do
    r = [0.125, 0.25, 1, 2].choose
    n = (scale :e3, :minor).choose
    co = rrand(30, 100)
    play n, release: r, cutoff: co
    sleep s
  end
end
live_loop :drums do
  use_random_seed 2001
  16.times do
    r = rrand(0.5, 10)
    sample :drum_bass_hard, rate: r, amp: rand
    sleep 0.125
  end
end
```
