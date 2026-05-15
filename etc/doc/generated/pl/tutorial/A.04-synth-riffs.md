A.4 Riffy Syntezatorów

# Riffy Syntezatorów

Niezależnie od tego, czy mamy do czynienia z niepokojącym dryftem dudniących oscylatorów, czy rozstrojonymi uderzeniami dźwięków piły przeszywającej miks, syntezator wiodący odgrywa kluczową rolę w każdym utworze muzyki elektronicznej. W ostatniej edycji tego samouczka z poprzedniego miesiąca omówiliśmy, w jaki sposób możemy kodować nasze bity. W bieżącym numerze omówimy zaś kodowanie trzech podstawowych części riffu syntezatorem - tembr (barwa dźwięku), melodię i rytm.

OK, czas żebyś włączył Twojego Raspberry Pi, odpal Sonic Pi w wersji 2.6+ i zróbmy jakiś hałas!


## Możliwości związane z Barwą Dźwięku

Esencją dowolnego riffu opartego o syntezatory jest zmienianie i granie z różnymi barwami dźwięku. W Sonic Pi możemy kontrolować barwę dźwięku na dwa sposoby - wybierając różne syntezatory dla uzyskania diametralnej zmiany oraz poprzez ustawianie różnych wartości opcji danego syntezatora dla otrzymania bardziej subtelnych zmian. Możemy również używać efektów (FX), ale to temat na osobny artykuł...

Spróbujmy stworzyć prostą żywą pętlę, w której będziemy zmieniać aktualny syntezator w trakcie gry:

```
live_loop :timbre do
  use_synth (ring :tb303, :blade, :prophet, :saw, :beep, :tri).tick
  play :e2, attack: 0, release: 0.5, cutoff: 100
  sleep 0.5
end
```

Spójrzmy na powyższy kawałek kodu. Cykamy tu przez pierścień z nazwami syntezatorów (iterując w kółko po wszystkich syntezatorach z listy). Przekazujemy nazwy kolejno wybieranych syntezatorów do funkcji `use_synth`, co powoduje zmianę aktualnego syntezatora przy każdym kolejnym przebiegu żywej pętli. Ponadto gramy nutę `:e2` (dźwięk E w drugiej oktawie) z czasem zanikania amplitudy ustawionym na 0.5 uderzenia (przy domyślnym BPM równym 60 jest to pół sekundy) oraz z opcją `cutoff:` ustawioną na 100.

Zauważ, że każdy z syntezatorów ma zupełnie inne brzmienie, pomimo że wszystkie grają tę samą nutę. Spróbuj teraz poeksperymentować i pobaw się! Zmień czas zanikania amplitudy (opcja release) na wyższe i mniejsze wartości. Na przykład zmień wartości dla opcji `attack:` i `release:`, aby zobaczyć, jak wielki wpływ na dźwięk mają różne czasy wejścia/wyjścia. Na koniec zmień opcję `cutoff:`, aby usłyszeć, jak znaczące na barwę dźwięku ma ustawianie różnych wartości odcięcia (najlepsze są wartości pomiędzy 60 a 130). Widzisz, jak wiele różnych dźwięków możesz stworzyć, zmieniając tylko kilka wartości? Gdy już uda Ci się to opanować, udaj się na zakładkę Syntezatory znajdującą się w systemie pomocy. Zobaczysz tam pełną listę syntezatorów oraz wszystkie indywidualne opcje, jakie są dostępne dla każdego z nich. Powinno dać Ci to wyobrażenie, jak wiele możliwości jest w zasięgu Twoich palców.

## Barwa dźwięku

Tembr to nic innego jak wyszukane słowo dla opisania, jakie jest brzmienie dźwięku. Jeśli zagrasz tę samą nutę z innym instrumentem takim jak skrzypce, gitara czy fortepian, wysokość dźwięku (jak wysoki lub niski jest dźwięk) będzie taka sama, za to barwa będzie inna. Ta właściwość dźwięku - ta cecha, która pozwala ci rozróżnić pianino od gitary, to tembr.


## Kompozycja Malodyczna

Kolejnym aspektem ważnym dla naszego syntezatora wiodącego jest wybór nut, które chcemy zagrać. Jeśli posiadasz aktualnie fajny pomysł, wtedy możesz po prostu stworzyć pierścień (ring) z Twoich nut i cykać przez nie w kółko:

```
live_loop :riff do
  use_synth :prophet
  riff = (ring :e3, :e3, :r, :g3, :r, :r, :r, :a3)
  play riff.tick, release: 0.5, cutoff: 80
  sleep 0.25
end
```
    
Zdefiniowaliśmy tutaj naszą melodię za pomocą pierścienia, który zawiera w sobie zarówno nuty takie jak `:e3`, jak i przerwy reprezentowane za pomocą symbolu `:r`. Następnie wykorzystujemy polecenie `.tick`, żeby przechodzić kolejno przez każdą z nut, co daje nam powtarzalny riff.

## Melodia Automatyczna

Nie zawsze łatwo jest wymyślić ciekawy riff od zera. Zamiast tego często dużo łatwiej jest poprosić Sonic Pi, aby wybrał dla nas losowy riff i użyć tego, który spodoba się nam najbardziej. Aby to osiągnąć, potrzebujemy połączyć trzy rzeczy: pierścienie, losowość oraz losowe ziarna. Spójrzmy na poniższy przykład:

```
live_loop :random_riff do
  use_synth :dsaw
  use_random_seed 3
  notes = (scale :e3, :minor_pentatonic).shuffle
  play notes.tick, release: 0.25, cutoff: 80
  sleep 0.25
end
```

Dzieje się tutaj kilka rzeczy - przyjrzyjmy się im po kolei. Na samym początku określamy, że używamy 3-go zestawu losowych ziaren. Co to znaczy? Otóż dość przydatną rzeczą jest możliwość ustawienia określonej wartości dla tych ziaren, dzięki czemu możemy przewidzieć, jaka będzie kolejna losowa wartość. Przy każdym obiegu żywej pętli będzie to taki sam zestaw wartości, jaki ustawiliśmy na początku - zestaw nr 3! Kolejną przydatną rzeczą, którą warto wiedzieć, jest to, że tasowanie pierścienia z nutami działa w taki sam sposób. W powyższym przykładzie prosimy w gruncie rzeczy o 'trzecie rozdanie' ze standardowej listy tasowań - które za każdym jest tym samym rozdaniem, gdyż za zawsze tuż przed tasowaniem ustawiamy losowe ziarno na tę samą. Koniec końców tykamy kolejno przez nasze potasowane nuty, aby zagrać riff.

A teraz moment, w którym zaczyna się zabawa. Jeśli zmienimy wartość losowego ziarna na inną liczbę, powiedzmy 3000, otrzymamy zupełnie inne rozdanie nut. Dzięki temu możemy teraz bardzo łatwo odkrywać nowe melodie. Po prostu wybieramy listę nut, które chcemy przetasować (gamy są tutaj dobrym punktem wyjścia), a następnie bierzemy wartość ziarna, według jakiej chcemy tasować. Jeśli dana melodia nam się nie spodoba, wystarczy zmienić jedną z tych dwóch rzeczy i spróbować ponownie. Teraz wystarczy, że będziesz powtarzał te kroki do momentu, aż usłyszysz coś, co Ci się spodoba!


## Pseudo Losowość

Losowość Sonic Pi tak naprawdę wcale nie jest losowa tylko pseudo losowa. Wyobraź sobie, że rzucasz kostką do gry 100 razy i zapisujesz wynik każdego rzutu na kartce papieru. Sonic Pi posiada odpowiednik takiej listy wyników, której używa, kiedy prosisz o losową wartość. Zamiast rzucania prawdziwą kostką do gry, po prostu wybiera kolejne wartości z listy. Ustawianie losowego ziarna jest po prostu skokiem do konkretnego miejsca w tej liście.
 
## Znajdź swój Rytm

Kolejnym ważnym aspektem naszego riffu jest rytm - kiedy grać daną nutę, a kiedy nie. Jak widzieliśmy wcześniej, możemy użyć symbolu `:r` w naszych pierścieniach, aby wypełnić pozostałości. Kolejnym bardzo skutecznym sposobem jest użycie rozpiętości - nauczymy się tego w dalszej części poradnika. Dzisiaj użyjemy losowości, która pomoże nam znaleźć nasz rytm. Zamiast grać każdą nutę, możemy użyć warunku logicznego, aby zagrać ją z zadanym prawdopodobieństwem. Rzućmy okiem:

```
live_loop :random_riff do
  use_synth :dsaw
  use_random_seed 30
  notes = (scale :e3, :minor_pentatonic).shuffle
  16.times do
    play notes.tick, release: 0.2, cutoff: 90 if one_in(2)
    sleep 0.125
  end
end
```

Bardzo przydatną i wartą poznania funkcją jest `one_in`. Zwraca ona prawdę (`true`) lub fałsz (`false`) z określonym prawdopodobieństwem. W naszym przypadku używamy wartości 2 więc przeciętnie raz na każde 2 wołania funkcji `one_in` zostanie zwrócona wartość `true` (prawda). Innymi słowy, przez 50% czasu będzie zwracała `true`. Użycie wyższych wartości spowoduje, że wartość `false` zacznie być zwracana częściej, co wprowadzi więcej wolnej przestrzeni do naszego riffu.

Zauważ, że wraz z poleceniem `16.times` dodaliśmy tutaj pewien rodzaj iteracji. Zrobiliśmy tak, ponieważ chcemy tylko zresetować losowe ziarno co każde 16 nut, tak żeby nasz rytm powtarzał się co każde 16 uderzeń. Nie wpłynie to na tasowanie, ponieważ dzieje się to od razu, jak tylko ziarno jest ustawione. Możemy użyć rozmiaru iteracji do zmiany długości trwania rytmu. Spróbuj zmienić liczbę 16 na 8 lub nawet 4 czy 3 i zobacz, jak wpływa to na rytm naszego riffu.

## Połączenie tego wszystkiego w jedną całość

No dobrze, to spróbujmy teraz wykorzystać wszystko, czego się nauczyliśmy w naszym finalnym przykładzie. Do zobaczenia w następnym odcinku!

```
live_loop :random_riff do
  #  uncomment to bring in:
  #  synth :blade, note: :e4, release: 4, cutoff: 100, amp: 1.5
  use_synth :dsaw
  use_random_seed 43
  notes = (scale :e3, :minor_pentatonic, num_octaves: 2).shuffle.take(8)
  8.times do
    play notes.tick, release: rand(0.5), cutoff: rrand(60, 130) if one_in(2)
    sleep 0.125
  end
end
 
live_loop :drums do
  use_random_seed 500
  16.times do
    sample :bd_haus, rate: 2, cutoff: 110 if rand < 0.35
    sleep 0.125
  end
end
 
live_loop :bd do
  sample :bd_haus, cutoff: 100, amp: 3
  sleep 0.5
end
```
