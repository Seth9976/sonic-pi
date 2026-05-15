A.19 Design de sunet - Sinteza substractiva

# Sinteza substractiva

Acesta este al doilea articol dintr-o serie referitoare la felul in care poti dolosi Sonic Pi pentru design de sunet. Luna trecuta am vorbit despre sinteza aditiva care inseamna pur si simplu redarea mai multor sunete in acelasi timp pentru ca crea un sunet combinat. De exemplu, putem combina diferite sintetizatoare, sau chiar sunete de la acelasi sintetizator la intaltimi diferite pentru a crea un sunet nou, complex, pornind de la ingrediente simple. Luna aceasta vom descoperi o noua tehnica, numita *sinteza substractiva* care inseamna ca pornim de la un sunet complex si extragem parti din el pentru a crea ceva nou. Aceasta tehnica este asociata de obicei cu sunetul sintetizatoarelor analogice din anii '60 si '70, dar si cu recenta revigorare a sintetizatoarelor analogice modulare prin standarde populare, cum ar fi Eurorack.

Desi suna ca o tehnica avansata si complicata, Sonic Pi face utilizarea ei surprinzator de facila. Sa intram direct in subiect.

## Semnal sursa complex

Pentru ca un sunet sa fie potrivit pentru sinteza substractiva, trebuie sa fie destul de bogat si interesant. Asta nu inseamna ca avem nevoie de ceva extrem de complex - de fapt, e suficienta o unda standard dreptunghiulara (`:square`) sau triunghiulara (`:saw`):

```
synth :saw, note: :e2, release: 4
```

Observi ca sunetul este deja destul de interesant si contine mult frecvente diferite mai sus de ':e2' (al doilea Mi la un pian) care se combina pentru a crea timbrul. Daca nu intelegi mare lucru din asta, incearca sa il compari cu un ':beep':

```
synth :beep, note: :e2, release: 4
```

Cum sintetizatorul ':beep' este o unda simpla sinusoidala, vei auzi un sunet mult mai pur, doar ':e2' fara nimic din sunetele mai inalte si mai bazaitoare care se aud la ':saw'. Cu acest zumzet care face diferenta fata de o unda sinusoidala ne putem juca pentru a aplica sinteza substractiva.

## Filtre

Odata ce avem sunetul brut, urmatorul pas este sa-l trecem printr-un filtru care il va modifica scotand sau atenuand anumite parti din el. Unul dintre filtrele cele mai folosite in sinteza substractiva este ceea ce se numeste filtrul trece-jos. Acesta permite componentelor joase din sunet sa treaca, dar le scoate sau le atenueaza pe cele inalte. Sonic Pi are un sistem de efecte (FX) foarte simplu dar foarte puternic care include un filtru trece-jos, numit ':lpf' (low-pass filter). Sa ne jucam cu el:

```
with_fx :lpf, cutoff: 100 do
  synth :saw, note: :e2, release: 4
end
```

Daca asculti cu atentie vei observa ca o parte din zumzet a disparut. De fapt, toate frecventele din sunet mai sus de nota '100' au fost reduse sau eliminate si au ramas in sunet doar cele mai joase. Incearca sa schimbi aceasta valoare de 'cutoff:' la o nota mai joasa, sa zicem '70', apoi '50' si apoi compara sunetele.

Desigur, ':lpf' nu este singurul filtru pe care il poti folosi pentru a modifica semnalul sursa. Un alt efect important care poate fi aplicat in Sonic Pi este filtrul trece-sus, numit ':hpf' (high pass filter). Acesta actioneaza invers decat ':lpf' prin aceea ca lasa sa treaca partile inalte ale sunetului si le taie pe cele joase.

```
with_fx :hpf, cutoff: 90 do
  synth :saw, note: :e2, release: 4
end
```

Observi ca sunetul a devenit mai zumzaitor si mai strident prin eliminarea sunetelor de frecventa joasa. Joaca-te cu valoarea de cutoff - observa cum valorile mai joase lasa sa treaca mai mult din partile de bass originale, iar valorile mai mari fac sunetul mai "subtire" si mai linistit.

## Low Pass Filter

![Varying amounts of low pass filtering](../../../etc/doc/images/tutorial/articles/A.19-subtractive-synthesis/subtractive-synthesis-waveforms.png)

Filtrul trece-jos este o parte foarte importanta a fiecarui instrument de sinteza substractiva si merita sa aruncam o privire mai atenta asupra felului in care functioneaza. Aceasta diagrama arata aceeasi unda sonora (sintetizatorul ':prophet') filtrata in mod diferit. In partea de sus, sectiunea A arata unda audio fara filtrare. Se observa ca forma de unda este foarte ascutita si contine multe variatii bruste. Aceste unghiuri ascutite produce bazaitul de tonalitate inalta din sunet. Sectiunea B arata cum actioneaza filtrul trece-jos - se observa ca unda este mai putin ascutita si mai rotunjita fata de cea dinainte. Asta inseamna ca sunetul va avea mai putine frecvente inalte si va suna mai cuminte. Sectiunea C arata filtrul trece-jos cu o frecventa de taiere destul de coborata - asta inseamna ca si mai multe dintre frecventele inalte au fost eliminate iar unda va fi si mai rotunjita. In fine, poti observa ca marimea undei (care reprezinta amplitudinea), scade de la A la C. Sinteza substractiva functioneaza prin elimnarea de parti din semnal, ceea ce inseamna ca amplitudinea globala scade cu atat mai mult cu cat filtrarea este mai intensa.


## Flitrarea modulata

Pana acum am produs sunete destul de statice. Cu alte cuvinte, sunetul nu se schimba in niciun fel pe toata durata sa. Uneori ai putea avea nevoie de o variatie a sunetului pentru a da viata timbrului. Un mod de a realiza acest lucru este sa aplici o filtrare modulata - adica sa schimbi parametrii filtrului in timp. Din fericire, Sonic Pi iti ofera unelte puternice pentru a manipula parametrii efectelor in timp. De exemplu, poti sa alegi un timp de variatie pentru fiecare parametru modulabil pentru a spune cat timp trebuie sa treaca pentru ca valoarea curenta sa se modifice liniar (slide) in valoarea tinta:

```
with_fx :lpf, cutoff: 50 do |fx|
  control fx, cutoff_slide: 3, cutoff: 130
  synth :prophet, note: :e2, sustain: 3.5
end
```

Sa vedem ce se petrece aici. Mai intai, pornim un flitru trece-jos cu o valoare foarte coborata ('50') pentru 'cutoff:'. Totusi, prima linie se termina in mod ciudat cu '|fx|'. Aceasta reprezinta o parte optionala din sintaxa pentru 'with_fx' care iti permite sa numesti si sa controlezi direct efectele care ruleaza. Linia 2 face exact acest lucru si controleaza efectul stabilindu-i timpul de glisare ('cutoff_slide') la 3 si valoarea tinta pentru 'cutoff:' la '130'. Valoarea pentru 'cutoff:' pentru acest efect va creste liniar de la '50' la '130' pe durata a 3 batai. La final vom proni un semnal sursa de la un sintetizator pentru a putea auzi efectul filtrului trece-jos modulat.


## Sa punem totul cap la cap

Acesta este doar un exemplu simplu pentru ce se poate obtine folosind filtrele pentru modificarea unor sunete. Incearca sa te joci cu multitudinea de efecte incluse in Sonic Pi si vezi ce sunete cool poti crea. Daca sunetul tau pare prea monoton, aminteste-ti ca poti modula parametrii sai pentru a ii da viata.

Sa incheiem creand o functie care va reda un sunet nou creat prin sinteza substractiva. Incearca sa-ti dai seama singur ce se petrece aici, iar daca esti un utilizator Sonic Pi avansat, spune-mi daca stii de ce am inglobat totul intr-un apel 'at' (te rog sa trimiti mesajul la @samaaron pe Twitter).

```
define :subt_synth do |note, sus|
  at do
    with_fx :lpf, cutoff: 40, amp: 2 do |fx|
      control fx, cutoff_slide: 6, cutoff: 100
      synth :prophet, note: note, sustain: sus
    end
    with_fx :hpf, cutoff_slide: 0.01 do |fx|
      synth :dsaw, note: note + 12, sustain: sus
      (sus * 8).times do
        control fx, cutoff: rrand(70, 110)
        sleep 0.125
      end
    end
  end
end
subt_synth :e1, 8
sleep 8
subt_synth :e1 - 4, 8
```
