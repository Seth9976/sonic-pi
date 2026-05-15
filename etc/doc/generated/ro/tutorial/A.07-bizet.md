A.7 Beat-uri Bizet

# Beat-uri Bizet

Dupa scurta noastr incursiune in fantastica lumea a programarii Minecraft folosind Sonic Pi de luna trecuta, sa revenim la muzica. Astazi vom aduce o bucata de dans dintr-o opera clasica direct in secolul 21 folosind puterea codului.

## Scandalos si tulburator

Sa calatorim inapoi in timp in anul 1875. Un compozitor pe nume Bizet tocmai a terminat ultima sa opera, Carmen. Din pacate, la fel ca in cazul multor altor piese incitante si neobisnuite, lumea nu a apreciat-o la inceput deoarece era scandalos de diferita. In mod tragic, Bizeta a murit cu zece ani inainte ca opera sa obtine un succes international imens si sa devina una dintre cele mai faimoase si cele mai frecvent interpretate opere din toate timpurile. Ca un semn de compasiune sa luam una dintre temele principale din Carmen si sa o convertim intr-un format modern care este de asemenea scandalos si diferit pentru majoritatea oamenilor din vremea nostra - muzica programata live!

## Decodarea Habanerei

Sa incercam sa scriem cod pentru intreaga opera ar fi o provocare serioasa pentru acest tutorial, mai bine sa ne concentram asupra uneia dintre cele mai faimoase parti  - linia de bas din Habanera:

![Riff Habanera](../../../etc/doc/images/tutorial/articles/A.07-bizet/habanera.png)

S-ar putea sa ti se para ca e in pasareasca daca nu ai studiat notatia muzicala inca. Totusi, ca programatori vedem notatia muzicala ca pe o alta forma de cod - doar ca reprezinta instructiuni pentru un muzician, nu pentru un computer. Trebuie deci sa gasim o cale sa o decodam.

## Notele

Notele sunt aranjate de la stanga la dreapta, la fel cu cuvintele din aceasta revista, dar au inaltimi diferite. *Inaltimea pe portativ reprezinta tonalitatea notei*. Cu cat e mai sus nota pe portativ, cu atat tonalitatea notei va fi mai sus.

In Sonic Pi stim deja sa modificam tonalitatea notei - fie folosim numere mai mari sau mai mici, cum ar fi 'play 75' si 'play 80', fie folosim numele notelor: 'play: E' si 'play:F'. Din fericire, fiecare dintre pozitiile verticale de pe portativ reprezinta o anumita nota. Priveste acest tabel util pentru cautare rapida:

![Notele](../../../etc/doc/images/tutorial/articles/A.07-bizet/notes.png)

## Pauzele

Partiturile muzicale reprezinta un tip de cod foarte bogat si expresiv capabil sa comunice multe lucruri. Nu ar trebui deci sa fie o surpriza ca partiturile pot sa-ti spuna nu doar ce note sa canti, ci si cand *nu* trebuie sa canti nicio nota. In programare acest lucru este echivalent cu folosirea 'nil' sau 'null' - absenta unei valori. Cu alte cuvinte, a nu canta nicio nota inseamna absenta unei note.

Daca privesti cu atentie partitura vei observa ca este de fapt o combinatie de puncte negre cu linii care reprezinta notele ce vor fi redate si niste chesti serpuite care reprezinta pauzele. Din fericire Sonic Pi are o reprezentare foarte comoda pentru pauze: ':r', deci daca dam comanda: 'play :r' va reda de fapt liniste... Putem de asemenea sa scriem 'play :rest', 'play nil' sau 'play false' care sunt toate moduri echivalente de a reprezenta pauzele.

## Ritmul

Mai este un ultim lucru pe care trebuie sa inveti sa-l decodezi in notatie - tempoul pentru note. In imaginea initiala vei vedea ca notele sunt conectate cu niste linii groase. A doua nota are doua astfel de linii, ceea ce inseamna ca tine o saisprezecime de masura. Celelalte note au o singura linie, ceea ce inseamna ca dureaza o optime. Pauza are si ea doua linii serpuite care inseamna ca dureaza tot o saisprezecime de masura.

Cand incercam sa decodam si sa exploram efectul unor modificari cel mai bine ar fi sa incercam sa facem totul cat mai asemanator si sa incercam sa vedem ce legaturi putem face. De exemplu, cand rescriem partitura noastra in saisprezecimi poti vede cum se transforma intr-o secventa draguta de note si pauze.

![Riff-ul Habanera 2](../../../etc/doc/images/tutorial/articles/A.07-bizet/habanera2.png)

## Rescrierea bucatii Habanera

Suntem acum in masura sa transpunem aceasta line de bas in Sonic Pi. Sa codam aceste note si pauze intr-o lista circulara:

```
(ring :d, :r, :r, :a, :f5, :r, :a, :r)
```
    
Sa vedem cum suna. Pune asta intr-o bucla live si fa o parcurgere ciclica:

```
live_loop :habanera do
  play (ring :d, :r, :r, :a, :f5, :r, :a, :r).tick
  sleep 0.25
end
```
    
Excelent, acest riff inconfundabil capata viata in difuzoarele tale. A fost foarte greu sa ajungem aici, dar a meritat - bate palma!
    
## Sintetizatoare de atmosfera

Avem linia de bas, sa recream o parte din atmosfera scenei de opera. Un sintetizatot care merita incercat este ':blade' care este un sintetizator in genul anilor 80. Sa-l incercam trecand nota ':d' de la inceput printr-un slicer si un efect reverb:

```
live_loop :habanera do
  use_synth :fm
  use_transpose -12
  play (ring :d, :r, :r, :a, :f5, :r, :a, :r).tick
  sleep 0.25
end
with_fx :reverb do
  live_loop :space_light do
    with_fx :slicer, phase: 0.25 do
      synth :blade, note: :d, release: 8, cutoff: 100, amp: 2
    end
    sleep 8
  end
end
```

Acum, incearca alte note in linia de bas: ':a' si ':f5'. Tine minte, nu e nevoie sa apesi pe Stop, doar modifici codul in timp ce se aude muzica si apesi pe Run din nou. De asemenea, incearca diferite valori pentru parametru 'phase:' al slicer-ului, cum ar fi `0.5`, `0.75` si `1`.

## Sa punem totul cap la cap

La final, sa combinam toate aceste idei intr-un nou remix dupa Habanera. Poate ai observat ca am inclus o alta parte din linia de bas sub forma de comentariu. Dupa ce ai copiat totul intr-un buffer nou apasa Run si asculta compozitia. Acum, fara sa apesi pe stop, sterge simbolul de comentariu '#' din fata celei de a doua linii si apasa Run din nou - nu e minunat? Acum poti sa-l modifici cum vrei si sa te distrezi.

```
use_debug false
bizet_bass = (ring :d, :r, :r, :a, :f5, :r, :a, :r)
#bizet_bass = (ring :d, :r, :r, :Bb, :g5, :r, :Bb, :r)
 
with_fx :reverb, room: 1, mix: 0.3 do
  live_loop :bizet do
    with_fx :slicer, phase: 0.125 do
      synth :blade, note: :d4, release: 8,
        cutoff: 100, amp: 1.5
    end
    16.times do
      tick
      play bizet_bass.look, release: 0.1
      play bizet_bass.look - 12, release: 0.3
      sleep 0.125
    end
  end
end
 
live_loop :ind do
  sample :loop_industrial, beat_stretch: 1,
    cutoff: 100, rate: 1
  sleep 1
end
 
live_loop :drums do
  sample :bd_haus, cutoff: 110
  synth :beep, note: 49, attack: 0,
    release: 0.1
  sleep 0.5
end
```
