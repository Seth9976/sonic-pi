A.17 Comprimarea/Intinderea esantioanelor

# Comprimarea/Intinderea esantioanelor

Cand cineva ia contact prima data cu Sonic PI, unul dintre primele lucruri pe care le descopera este cat de simplu este sa redai sunete pre-inregistrate folosind functia 'sample'. De exemplu, poti reda in bucla un sunet de toba, sa auzi vocile unui cor sau chiar sa asculti niste efecte obtinute prin manipularea discurilor de vinil printr-o singura linie de cod. Totusi, sunt multi cei care nu realizeaza ca pot modifica viteza la care este redat esantionul pentru a obtine niste efecte interesante si a controla mai bine sunetul inregistrat. Deci, porneste Sonic Pi si hai sa incepem sa ne jucam cu viteza esantioanelor!

## Incetinirea esantioanelor

Pentru a modifica viteza de redare a esantionului, trebuie sa folosim parametrul 'rate:'

```
sample :guit_em9, rate: 1
```
    
Daca specificam valoarea '1' pentru 'rate:', esantionul va fi redat la viteza normala. Daca vrem sa fie redat cu o viteza injumatatita, folosim valoarea '0.5' pentru 'rate:':


```
sample :guit_em9, rate: 0.5
```
    
Observi ca aceasta va avea doua efecte. In primul rand esantionul va contine sunete mai joase si in al doilea rand redarea sa va dura de doua ori mai mult (vezi in bara laterala o explicatie pentru acest lucru). Putem alege valori din ce in ce mai mici pentru 'rate:', pana spre '0', '0.25' avand viteza un sfert din cea initiala, '0.1' o zecime, etc. Incearca sa te joci cu niste valori mai joase si sa vezi cum sunetul e transforma intr-un huruit cu tonuri grave.

## Redarea esantioanelor cu viteza marita

In afara de a face sunetul sa dureze mai mult si sa fie mai grav, putem sa il facem si mai scurt, dar mai acut, folosind valori mai mare pentru 'rate:'. Sa ne jucam cu un sunet de toba de data asta. Mai intai, sa ascultam cum suna la viteza normala:

```
sample :loop_amen, rate: 1
```


Acum, sa il redam putin mai rapid:

```
sample :loop_amen, rate: 1.5
```
    
Ha! Tocmai am schimbat genul muzical din old-skool techno in jungle. Observi ca tonalitatea pentru fiecare bataie de toba este mai inalta si tot ritmul este mai rapid, Acum, incearca viteze chiar mai mari si vezi cat de scurt si inalt poti face sunetul de toba. De exemplu, daca folosesti '100' pentru 'rate:', sunetul se transforma intr-un click!

## Marsarier

Sunt sigur ca multi dintre voi va ganditi acum... "dar daca as folosi o valoare negativa pentru viteza?". Buna intrebare! Sa ne gandim putin la acest lucru. Daca parametrul 'rate:' semnifica viteza cu care sunetul este redat, '1' fiind viteza normala, '2' fiind viteza dubla, '0.5' viteza injumatatita, '-1' trebuie sa insemne redare inversa! Sa incercam cu un sunet de toba. Mai intai, reda-l la viteza normala:

```
sample :elec_filt_snare, rate: 1
```
    
Acum, reda-l de la coada la cap:

```
sample :elec_filt_snare, rate: -1
```
    
Desigur, il poti reda invers cu viteza dubla folosind valoarea '-2' sau cu viteza injumatatita folosind valoarea '-0.5' pentru 'rate:'. Acum joaca-te cu diferite viteze negative si distreaza-te. Este amuzant in special daca folosesti esantionul ':misc_burp'!


## Esantion, Viteza si Tonalitate [Bara laterala]

Unul dintre efectele modificarii vitezei esantioanelor este ca o redare mai rapida produce sunete mai inalte, iar una mai lenta duce la sunete mai joase. Acest efect poate fi intalnit in viata de zi cu zi cand mergi cu bicicleta si treci pe langa un semafor cu semnalizare sonora - cand te apropii de sursa sunetului tonalitatea este ma inalta decat atunci cand te indepartezi - efectul Doppler. De ce se intampla asta?

Sa analizam un sunet simplu reprezentat ca o unda sinusoidala. Daca folosim un osciloscop pentru a vizualiza sunetul vom vedea ceva ca in figura A. Daca reprezentam un sunte mai inalt cu o octava, vom vedea figura B, iar cu o octava mai jos va arata ca in figura C. Observam ca undele pentru notele mai inalte sunt ai compacte, iar undele pentru notele mai joase sunt mai extinse.

Un esantion pentru un sunet reprezinta doar un set de numere (coordonate x, y) care atunci cand sunt afisate grafic redeseneaza curbele initiale. Vezi figura D unde fiecare cerc reprezinta o coordonata. Pentru a transforma aceste coordonate inapoi in sunet, computerul parcurge fiecare coordonata x si trimite valoarea y corespunzatoare la difuzoare. Interesant este ca viteza cu care computerul parcurge valorile pentru x nu trebuie sa fie aceeasi cu cea cu care au fost inregistrate. Deci, cand computerul parcurge valorile pentru x mai rapid decat la inregistrare, efectul va fi de inghesuire a cercurilor, ceea ce va duce la un sunet mai inalt. Sunetul va fi deasemenea mai scurt, deoarece vom parcurge aceste valori mai repede. Acest lucru este aratat in figura E.

Un ultim lucru pe care trebuie sa-l stii este ca un matematician numit Fourier a demonstrat ca fiecare sunet este de fapt format dintr-o multime de unde sinusoidale combinate. Ca urmare, cand comprimam sau extindem un sunet inregistrat, de fapt comprimam sau extindem in acelasi timp mai multe unde sinusoidale in modul descris anterior.

## Modificarea tonalitatii

Asa cum am vazut, folosind o viteza de redare mai mare vom obtine un sunet cu o tonalitate mai inalta, iar o viteza mai mica va crea un sunet mai grav. O regula simpla spune ca dublarea vitezei va duce la un sunet cu o octava mai sus si invers, injumatatirea vitezei duce la un sunet cu o octava mai jos. Aceasta inseamna ca pentru esantioanele melodice redarea lor in paralel la viteza normala, dubla si injumatatita va suna chiar interesant:

```
sample :bass_trance_c, rate: 1
sample :bass_trance_c, rate: 2
sample :bass_trance_c, rate: 0.5
```
    
Totusi, ce ar trebui sa facem daca am dori doar sa schimbam viteza astfel incat sa crestem tonalitatea cu un semiton (cu o nota mai sus la pian)? Sonic Pi face acest lucru foarte usor folosind parametrul 'rpitch:':

```
sample :bass_trance_c
sample :bass_trance_c, rpitch: 3
sample :bass_trance_c, rpitch: 7
```
    
Daca arunci o privire la jurnalul din partea dreapta, vei observa ca o valoare de '3' pentru 'rpitch:' corespunde unei viteze ('rate:') de `1.1892` iar o valoare `rpitch:` de `7` corespunde unui 'rate:' de `1.4983`. In fine, putem chiar combina parametrii `rate:` si `rpitch:`:

```
sample :ambi_choir, rate: 0.25, rpitch: 3
sleep 3
sample :ambi_choir, rate: 0.25, rpitch: 5
sleep 2
sample :ambi_choir, rate: 0.25, rpitch: 6
sleep 1
sample :ambi_choir, rate: 0.25, rpitch: 1
```
    

## Sa punem totul cap la cap

Sa aruncam o privire la o bucata simpla care combina aceste idei. Copiaza codul intr-un buffer gol, apasa play si asculta putin apoi foloseste-l ca punct de pornire pentru propria ta piesa. Vezi cat de distractiv este sa modifici viteza de redare a esantioanelor. Ca exercitiu suplimentar, incearca sa inregistrezi propriile sunete si joca-te cu viteza de redare pentru a vedea ce sunete interesante poti obtine.

```
live_loop :beats do
  sample :guit_em9, rate: [0.25, 0.5, -1].choose, amp: 2
  sample :loop_garzul, rate: [0.5, 1].choose
  sleep 8
end
 
live_loop :melody do
  oct = [-1, 1, 2].choose * 12
  with_fx :reverb, amp: 2 do
    16.times do
      n = (scale 0, :minor_pentatonic).choose
      sample :bass_voxy_hit_c, rpitch: n + 4 + oct
      sleep 0.125
    end
  end
end
```







