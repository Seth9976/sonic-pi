A.15 Pięć Technik Kodowania Na Żywo

# Pięć Technik Kodowania Na Żywo

W bieżącym numerze tego poradnika o Sonic Pi zobaczymy, w jaki sposób możesz zacząć traktować Sonic Pi jak prawdziwy instrument. Musimy zatem zacząć myśleć o kodzie w zupełnie nowy sposób. Osoby kodujące na żywo myślą o kodzie w sposób podobny do tego, w jaki skrzypkowie myślą o swoich smyczkach. W rzeczy samej tak jak skrzypek potrafi wykorzystać różne techniki smyczkowania, aby stworzyć różne dźwięki (długie powolne ruchy kontra szybkie krótkie pociągnięcia), tak samo my poznamy pięć podstawowych technik kodowania na żywo, które umożliwia Sonic Pi. Pod koniec tego artykułu będziesz mógł samodzielnie ćwiczyć na potrzeby swoich własnych występów przed publicznością.

## 1. Zapamiętaj Skróty

Pierwszą wskazówką w kodowaniu na żywo z Sonic Pi jest zaczęcie używania skrótów klawiszowych. Na przykład zamiast tracić cenny czas na sięganie po myszkę, przesuwanie ją na przycisk Run i kliknięcie, możesz po prostu nacisnąć jednocześnie przyciski `alt` i `r`. Będzie to dużo szybsze i dzięki temu wciąż będziesz miał palce na klawiaturze gotowe do kolejnej edycji kodu. Możesz poznać wszystkie skróty klawiszowe dotyczące przycisków na górze poprzez najechanie na nie myszką i poczekanie chwilę, aż pokaże się opis. Aby zobaczyć pełną listę skrótów klawiszowych, udaj się do sekcji 10.2 samouczka wbudowanego w Sonic Pi.

Jedną z fajniejszych rzeczy, które można zrobić podczas występów na żywo, jest nadanie Twoim ruchom finezji, gdy naciskasz skróty klawiszowe. Na przykład bardzo dobrą rzeczą jest zakomunikowanie publiczności, że zamierzasz wprowadzić zmianę - postaraj się więc ozdobić Twój ruch, gdy naciskasz `alt-r` dokładnie w taki sam sposób, w jaki zrobiłby to gitarzysta gdy uderza o struny, aby zagrać fantastyczny "akord mocy" (ang. power chord).

## 2. Układaj Swoje Dźwięki Manualnie

Teraz potrafisz już natychmiast uruchamiać kod za pomocą klawiatury, możesz więc od razu zacząć wykorzystywać tę umiejętność przy drugiej technice polegającej na ręcznym układaniu kolejnej warstwy Twojej kompozycji. Zamiast 'komponowania' przy wykorzystaniu wielu poleceń `play` i `sample` oddzielonych odwołaniami do `sleep`, będziemy mieli tylko jedno wywołanie polecenia `play`, które będziemy manualnie zmieniać, używając polecenia `alt-r`. Spróbujmy zatem! Wpisz poniższy kod do pustego bufora:

```
synth :tb303, note: :e2 - 0, release: 12, cutoff: 90
```

A teraz naciśnij przycisk `Run` i gdy dźwięk wciąż jeszcze gra, popraw kod, aby dorzucić cztery nuty poprzez zmianę kodu na następujący:


```
synth :tb303, note: :e2 - 4, release: 12, cutoff: 90
```

A teraz naciśnij ponownie przycisk `Run`, aby usłyszeć oba dźwięki grające w tym samym czasie. Dzieje się tak, ponieważ przycisk `Run` dostępny w Sonic Pi nie czeka w ogóle na zakończenie jakiegokolwiek poprzedniego kodu, lecz zamiast tego uruchamia nasz kod w tym samym czasie. Oznacza to, że możesz ręcznie w bardzo łatwy sposób nawarstwić wiele razy różne dźwięki z małymi lub dużymi zmianami pomiędzy każdym uruchomieniem. Na przykład spróbuj zmienić za jednym razem opcje `note:` i `cutoff:`, a następnie odpalić kod ponownie.


Możesz spróbować tej techniki z długimi abstrakcyjnymi samplami. Na przykład:

```
sample :ambi_lunar_land, rate: 1
```

Spróbuj zacząć z wyłączonym samplem, a następnie sukcesywnie zmniejszaj o połowę wartości opcji `rate:` pomiędzy kolejnymi naciśnięciami `Run`. Zacznij od `1`, potem `0.5`, `0.25` aż do `0.125` - sprawdź nawet jakieś liczby ujemne, na przykład `-0.5`. Spróbuj nawarstwić te dźwięki na siebie i zobacz, dokąd uda Ci się to doprowadzić. Na koniec spróbuj dodać jakiś efekt.

Kiedy występujesz na żywo, pracując w ten sposób z prostymi liniami kodu, to publiczność, dla której Sonic Pi jest czymś nowym, ma całkiem niezłą szansę na to, żeby śledzić to, co robisz i kod, który tworzysz, dzięki czemu mogą czytać docierający do nich dźwięk.


## 3. Opanuj Żywe Pętle (Live Loops)

Kiedy pracujemy z bardziej rytmiczną muzyką, uruchamianie wszystkiego z ręki przy utrzymaniu odpowiedniego tempa może być dosyć trudne, jeśli nie niemożliwe. Zamiast tego dużo lepiej jest skorzystać z żywej pętli `live_loop`. Pozwala Ci to na powtarzanie Twojego kodu, a jednocześnie masz możliwość edycji i przygotowania go na kolejny przebieg pętli. Ponadto będą one uruchomione w tym samym czasie co inne żywe pętle `live_loop` - oznacza to, że możesz połączyć je razem ze sobą oraz z manualnym uruchamianiem kodu. Zerknij na sekcję 9.2 wbudowanego samouczka, aby uzyskać więcej informacji o tym, jak możesz pracować z żywymi pętlami.

Kiedy występujesz na żywo, pamiętaj, aby korzystać z dostępnej dla polecenia `live_loop` opcji `sync:`. Pozwoli Ci to na naprawienie nieoczekiwanych pomyłek, które powodują zatrzymanie danego kawałka kodu ze względu na błąd. Jeśli posiadasz już opcję `sync:`, która wskazuje na inną istniejącą i poprawną pętlę `live_loop`, to możesz szybko naprawić błąd i ponownie uruchomić kod, co pozwoli na zresetowanie całej sytuacji bez utraty choćby jednego beatu.

## 4. Korzystaj z Głównego Miksera (Master Mixer)

Jednym z najpilniej strzeżonych sekretów Sonic Pi jest to, że posiada on główny mikser, przez który przepływają wszystkie dźwięki. Mikser ten posiada zarówno filtr niskich częstotliwości jak i filtr wysokich częstotliwości. Możesz więc bardzo łatwo wprowadzać globalne modyfikacje dźwięku. Dostęp do głównego miksera uzyskasz za pomocą funkcji `set_mixer_control!`. Przykładowo, kiedy został uruchomiony kod i odtwarzany jest dźwięk, wprowadź to do zapasowego bufora i naciśnij `Run`:

` set_mixer_control! lpf: 50 `

Po uruchomieniu tego kodu wszystkie nowe dźwięki będą miały zaaplikowany filtr niskich tonów i w związku z tym zostaną bardziej stłumione. Zauważ, że oznacza to, iż nowe wartości miksera zostaną takie same, dopóki nie zostaną ponownie zmienione. Jeśli jednak chcesz, możesz zawsze zresetować mikser do domyślnego poziomu poprzez użycie `reset_mixer!`. Kilkoma obecnie wspieranymi opcjami są: `pre_amp:`, `lpf:` `hpf:` oraz `amp:`. Aby zobaczyć pełną listę dostępnych parametrów, wystarczy, że zajrzysz do wbudowanej dokumentacji dostępnej dla polecenia `set_mixer_control!`.

Użyj opcji typu `*slide`, aby zmienić poziom wartości jednej lub wielu opcji w taki sposób, jakbyś używał do tego celu suwaka na mikserze. Posługując się przykładem, żeby powoli przesunąć na mikserze filtr niskich częstotliwości w dół z obecnej wartości do 30, użyj poniższego:

```
set_mixer_control! lpf_slide: 16, lpf: 30
```

Po tym przesunięciu możesz szybko wrócić do wyższej wartości za pomocą:

```
set_mixer_control! lpf_slide: 1, lpf: 130
```

Kiedy występujesz, często przydatne jest zachowanie czystego bufora do pracy z takim mikserem jak ten.

## 5. Praktyka

Najważniejszą techniką do nauki żywego kodowania jest praktyka. Tę metodę wykorzystuje wielu profesjonalnych muzyków - grają oni na swoich instrumentach przez kilka godzin dziennie, by stać się coraz to lepszymi twórcami. Trenowanie jest tak samo ważne zarówno dla osoby kodującej na żywo, jak i gitarzysty. Pozwala ono Twoim palcom na zapamiętanie pewnych wzorców, co umożliwia im następnie o wiele płynniejszą pracę. Praktyka daje również możliwości do odkrywania nowych dźwięków i składni kodów.

Podczas występów na żywo zauważysz, że im więcej ćwiczysz, tym łatwiej będzie Ci się rozluźnić na imprezie. Ponadto dzięki praktyce zdobędziesz sporo doświadczenia, które będziesz mógł potem wykorzystać. Także pomoże Ci zrozumieć, które typy zmian będą interesujące oraz czy będą dobrze współgrać z aktualnie grającymi dźwiękami.

## Połączenie tego wszystkiego w jedną całość

W tym miesiącu zamiast dawać Ci końcowy przykład, który bardzo dobrze wkomponowywał się w tematy omawiane w danym numerze, proponuję małe wyzwanie. Zobacz, czy jesteś w stanie spędzić cały tydzień, ćwicząc każdego dnia jedną z wymienionych technik. Na przykład jednego dnia ćwicz ręczne uruchamianie kodu, następnego spróbuj popracować z jakąś prostą żywą pętlą `live_loop`, a kolejnego dnia spróbuj pobawić się z głównym mikserem. Powtórz to raz jeszcze. Nie przejmuj się, jeśli na początku będziesz odczuwać, że idzie to jak po grudzie - po prostu staraj się ćwiczyć, a zanim się obejrzysz, zaczniesz kodować na żywo przed prawdziwą publicznością.
