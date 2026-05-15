A.15 Cinci tehnici de programare live

# Cinci tehnici de programare live

In tutorialul de luna aceasta vom vedea cum putem sa tratam Sonic Pi ca pe un instrument adevarat. Va trebui deci sa incepem sa ne gandim la cod intr-un mod complet diferit. Cei care programeaza live se gandesc la cod intr-un mod similar cu cel in care violonistul se gandeste la arcus. De fapt, asa cum violonistul poate aplica tehnici diferite de manuire a arcusului pentru a crea sunete diferite (miscari lungi si lente vs lovituri scurte si rapide), vom vedea ca Sonic Pi permite tehnici diferite de programare live dintre care vom analiza 5. La sfarsitul acestui tutorial vei putea incepe sa exersezi pentru propriile interpretari live.

## 1. Memorati scurtaturile de la tastatura (shortcuts)

Primul sfat pentru a putea programa live cu Sonic Pi este sa incepi sa folosesti shortcut-uri. De exemplu, in loc sa pierzi timp pretios ca sa iei mouse-ul, sa il muti deasupra butonului Run si sa faci click, poti apasa 'Alt' si 'r' in acelasi timp, ceea ce este mai rapid si iti tine degetele deasupra tastaturii, pregatite pentru ce ai de introdus. Poti descoperi care sunt scurtaturile pentru butoanele principale deplasand cursorul mouse-ului deasupra lor. Vezi sectiunea 10.2 a tutorialului inclus in program pentru lista completa de scurtaturi.

Cand interpretati, un lucru haios pe care il puteti face este sa adaugati un pic de stil prin miscarea mainilor cand apasati scurtaturile. De exemplu, este bine sa comunicati audientei cand va pregatiti sa faceti o schimbare - deci ornati putin miscarea cand apasati 'Alt-r' exact cum un chitarist ar face-o cand reda un acord de forta.

## 2. Stratificati manual sunetele

Acum puteti porni codul instantaneu cu ajutorul tastaturii si puteti folosi aceasta abilitate pentru a doua tehnica - stratificarea manuala a sunetelor. In loc sa 'compuneti' folosind o multime de apeluri 'play' si 'sample' separate de apeluri 'sleep' pentru pauze, vom avea un apel 'play' pe care il vom declansa manual folosind 'Alt-r'. Sa incercam. Scrie codul de mai jos intr-un buffer nou:

```
synth :tb303, note: :e2 - 0, release: 12, cutoff: 90
```

Acum, apasa 'Run' si, in timp ce sunetul este redat, schimba codul pentru a cobori patru note, modificandu-l in:


```
synth :tb303, note: :e2 - 4, release: 12, cutoff: 90
```

Acum, apasa 'Run' din nou, pentru a auzi ambele sunete redate in acelasi timp. Aceasta deoarece apasarea butonului 'Run' in Sonic Pi duce la executarea codului imediat, fara a astepta ca sunetul dinainte sa se termine. Asta inseamna ca poti adauga manual cu usurinta noi straturi sunetului cu modificari mai mici sau mai mari intre doua lansari in executie. De exemplu, incearca sa schimbi atat parametrul 'note:' cat si 'cutoff:', apoi reporneste.


Poti incerca aceasta tehnica si cu esantioane abstracte lungi. De exemplu:

```
sample :ambi_lunar_land, rate: 1
```

Incearca sa pornesti esantionul apoi injumatateste de fiecare data parametrul 'rate:' inainte de a apasa din nou pe 'Run', de la '1' la '0.5' la '0.25' la '0.125' si apoi chiar la valori negative cum ar fi '-0.5'. Asterne straturile de sunet unul peste altul si vezi la ce poti ajunge. Apoi, incearca sa adaugi efecte.

Cand interpretezi, lucrul cu linii simple de cod in modul acesta inseamna ca audienta care ia prima data contact cu Sonic Pi are o buna sansa sa urmareasca ce faci si sa faca legatura intre codul pe care il citesc si sunetele pe care le aud.


## 3. Stapaneste buclele live

Cand lucrezi cu muzica mai ritmata, poate fi adesea dificil sa pornesti totul manual si sa pastrezi sincronizarea. De aceea, e mai bine sa sa folosesti 'live_loop'. Aceasta asigura repetarea codului in timp ce iti da posibilitatea sa modifici codul pentru urmatoarea executie a buclei. De asemenea, se va executa in acelasi timp cu alte bucle live, ceea ce inseamna ca straturile de sunet vor fi sincronizate. Arunca o privire la sectiunea 9.2 din tutorial pentru mai multe informatii despre lucrul cu buclele live.

Cand interpretezi, tine minte sa folosesti parametrul 'sync:' al 'live_loop' pentru a permite revenirea dupa probleme de rulare care opresc bucla live din cauza unor erori. Daca ai deja parametrul 'sync:' legat de o alta bucla live valida, atunci poti sa remediezi cu usurinta eroarea si sa rulezi din nou codul pentru a reporni sunetul fara sa pierzi un beat.

## 4. Foloseste Mixerul principal

Unul dintre cele mai bine pastrate secrete ale Sonic Pi este ca are un mixer principal prin care trec toate sunetele. Mixerul are incorporate atat un filtru trece jos cat si unul trece sus, deci puteti face cu usurinta modificari globale asupra sunetului. Functionalitatile mixerului principal pot fi accesate folosind functia 'set_mixer_control!'. De exemplu, in timp ce se executa o bucata de cod care produce sunete, introdu comanda de mai jos intr-un buffer gol si apasa 'Run':

` set_mixer_control! lpf: 50 `

Dupa ce executi aceasta instructiune, tuturor sunetelor existente si viitoare li se va aplica un filtru trece jos si vor fi in consecinta mai inabusite. Retine ca aceasta inseamna ca valorile pe care le-am setat pentru mixer voi fi mentinute pana cand le schimbam din nou. Totusi, daca vrei, poti sa resetezi oricand mixerul la starea implicita cu 'reset_mixer!'. Cativa dintre parametrii suportati in acest moment sunt: `pre_amp:`, `lpf:` `hpf:` si `amp:`. Pentru lista completa, vezi documentatia pentru `set_mixer_control!`.

Foloseste parametrii '*_slide' ai mixerului pentru a face sa varieze una sau mai multe valori in timp. De exemplu, pentru a face ca filtrul trece jos sa coboare lin de la valoarea curenta la 30, foloseste:

```
set_mixer_control! lpf_slide: 16, lpf: 30
```

Poti apoi sa cresti rapid la o valoare mai ridicata cu:

```
set_mixer_control! lpf_slide: 1, lpf: 130
```

Cand interpretezi, este util sa pastrezi un buffer liber pentru a lucra cu mixerul in acest mod.

## 5. Exerseaza

Cea mai importanta tehnica pentru programarea live este exersarea. Caracteristica cea mai intalnita intre muzicienii profesionisti de toate tipurile este ca ei exerseaza cantatul la instrumentele lor - adesea multe ore pe zi. Exersarea este la fel de importanta pentru un programator live ca si pentru un chitarist. Repetitia permite degetelor tale sa memoreze anumite sabloane si modificari frecvente astfel incat sa le poti introduce de la tastatura si sa lucrezi cu ele mai fluent. Exersarea iti ofera de asemenea posibilitatea sa explorezi sunete si secvente de cod noi.

Cand interpretezi, vei descoperi ca iti va fi cu atat mai usor cu cat ai exersat mai mult. Exersarea iti va oferi de asemenea o experienta utila. Aceasta te va ajuta sa intelegi ce tipuri de modificari vor fi mai interesante si vor merge mai bine cu sunetele curente.

## Sa punem totul cap la cap

Luna aceasta, in loc sa iti dau un exemplu final care combina toate elementele discutate, mai bine sa ne distram stabilind o provocare. Vezi daca poti sa petreci o saptamana exersand cate una dintre aceste tehnici in fiecare zi. De exemplu, intr-o zi exersezi lansarea manuala a sunetelor, in ziua urmatoare incerci niste bucle live simple, iar in urmatoare te joci cu mixerul principal. Apoi repeti. Nu te ingrijoar daca simti ca esti lent si neindemanatic la inceput - continua sa exersezi si fara sa iti dai seama vei ajuge sa programezi live in fata unei audiente reale.
