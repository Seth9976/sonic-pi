A.18 Design de sunet - Sinteza aditiva

# Sinteza aditiva

Acesta este primul dintr-o scurta serie de articole despre cum sa utilizezi Sonic Pi pentru design de sunet. Vom face un tur rapid al catorva tehnici pe care le ai la dispozitie pentru a crea propriile sunete unice. Prima tehnica pe care o vom analiza se numeste *sinteza aditiva*. Poate suna complicat, dar daca explicam putin fiecare cuvant, va fi foarte usor de inteles. In primul rand, aditiv inseamna o combinatie de element, iar sinteza inseamna crearea de sunete. Sinteza aditiva inseamna deci nici mai mult nici mai putin decat *combinarea unor sunete existente pentru a crea unele noi*. Aceasta tehnica de sinteza dateaza de foarte mult timp - de exemplu orgile din evul mediu aveau o multime de tuburi care sunau diferit si care puteau fi activate sau dezactivate folosind parghii. Tragand de parghia pentru un anumit tub, acesta era 'adaugat in combinatie' facand sunetul mai bogat si mai complex. Acum, sa vedem cum putem trage de aceste parghii cu Sonic Pi.


## Combinatii simple

Sa incepem cu cel mai simplu sunet - unda sinusoidala pura:

```
synth :sine, note: :d3
```

Acum, sa vedem cum suna impreuna cu o unda rectangulara:

```
synth :sine, note: :d3
synth :square, note: :d3
```

Ai observat cum cele doua sunete s-au combinat intr-unul nou, mai bogat. Desigur, nu trebuie sa ne oprim aici, putem adauga cate sunete ne trebuie. Totusi, trebuie sa avem grija cate sunete suprapunem. Asa cum atunci cand amestecam vopsele pentru a obtine noi culori, daca punem prea multe culori vom obtine un fel de maroniu murdar, in mod asemanator, amestecarea prea multor sunete va crea un nou sunet lipsit de claritate.


## Amestecarea

Sa adaugam ceva care va face sunetul mai viu. Am putea folosi o unda triunghiulara cu o octava mai sus (pentru un sunet viu inalt), dar sa o redam la o amplitudine de '0.4' astfel incat doar va adauga ceva sunetului in loc sa-l acapareze cu totul:

```
synth :sine, note: :d3
synth :square, note: :d3
synth :tri, note: :d4, amp: 0.4
```

Acum, incearca sa creezi propriile tale sunete combinand 2 sau mai multe sintetizatoare in octave si cu amplitudini diferite. De asemenea, observa ca poti sa te joci cu fiecare dintre parametrii sintetizatoarelor pentru a modifica fiecare sunet sursa inainte de a-l adauga la mix pentru si mai multe combinatii de sunete.


## Dezacordarea

Pana acum, cand am combinat diferite sintetizatoare, fie am folosit aceeasi inaltime, fie am schimbat octava. Cum ar suna daca nu am respecta octavele ci am alege o nota putin mai inalta sau mai joasa? Sa incercam:

```
detune = 0.7
synth :square, note: :e3
synth :square, note: :e3 + detune
```

Daca decalam cele 2 unde rectangulare cu 0.7 note putem auzi ceva ce probabil nu suna corect, ci dezacordat - o nota 'gresita'. Totusi, pe masura ce ne apropiem de 0 va suna din ce in ce mai putin dezacordat, inaltimile celor doua unde devenind mai apropiate si mai asemanatoare. Incearca! Schimba valoarea pentru 'detune:' din '0.7' in '0.5' si asculta noul sunet. Incearca `0.2`, `0.1`, `0.05`, `0`. De fiecare data cand schimbi valoarea, asculta si incearca sa auzi cum se schimba sunetul. Observa ca un dezacord minor, cum ar fi cu '0.1' produce un sunet 'plin', cele doua inaltimi usor diferite interactionand in moduri interesante, adesea surprinzatoare.

Unele dintre sintetizatoarele incluse au deja parametrul 'detune:' care face exact acelasi lucru pentru un singur sintetizator. Joaca-te cu parametrul `detune:` pentru `:dsaw`, `:dpulse` si `:dtri`.


## Modelarea amplitudinii

O alta metoda prin care poti fasona sunetul este sa folosesti anvelope si parametri diferiti pentru fiecare sintetizator. De exemplu, aceasta iti va permite sa faci anumite parti ale sunetului mai percutante iar altele mai rasunatoare pentru un timp.

```
detune = 0.1
synth :square, note: :e1, release: 2
synth :square, note: :e1 + detune, amp: 2, release: 2
synth :gnoise, release: 2, amp: 1, cutoff: 60
synth :gnoise, release: 0.5, amp: 1, cutoff: 100
synth :noise, release: 0.2, amp: 1, cutoff: 90
```

In exemplul de mai sus am combinat un element zgomotos percutant alaturi de un huruit de fundal mai persistent. Am realizat asta in primul rand folosind doua sintetizatoare de zgomot cu valorii medii de cutoff (`90` si `100`) si timp de release scurt, alaturi de un zgomot cu release mai lung, dar cu o valoare de cutoff mai joasa (care face zgomotul mai putin clar si mai huruitor).

## Sa punem totul cap la cap

Sa combinam toate aceste tehnici pentru a vedea daca putem folosi sinteza aditiva pentru a re-crea un sunet de clopot simplu. Am impartit acest exemplu in 4 sectiuni. In primul rand avem sectiunea 'hit' (lovitura) care reprezinta partea de atac a sunetului de clopot - deci utilizeaza o anvelopa scurta (de exemplu un 'release:' de aproximativ '0.1'). Apoi avem sectiunea mai lunga a sunetului de clopot in care folosesc o unda sinusoidala. Observi ca adesea cresc nota cu '12' sau '24', ceea ce reprezinta numarul de note din una, respectiv doua octave. Am mai adaugat si doua unde sinusoidale joase, pentru a da sunetului niste bass si profunzime. La final, am folosit 'define' pentru a ingloba codul intr-o functie pe care o pot apoi folosi pentru a canta o melodie. Incearca sa canti propria melodie si sa modifici continutul functiei ':bell' pana cand obtii niste sunete super tari cu care sa te joci!

```
Canta o melodie cu noul tau clopot!
```
