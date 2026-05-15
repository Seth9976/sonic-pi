A.5 Acid Bass

# Acid Bass

Gdy spojrzymy wstecz na historię muzyki elektronicznej, nie sposób jest nie zauważyć ogromnego wpływu, jaki miał na nią malutki syntezator TB-303. Jest to sekretny składnik stojący za oryginalnym brzmieniem acid'owej linii basowej. Te klasycznie piszczące i chlupiące riffy basowe TB-303 można usłyszeć zarówno we wczesnej scenie Chicago House, jak i u bardziej współczesnych artystów takich jak Plastikman, Squarepusher czy Aphex Twin.

Interesujące jest, że firma Roland nigdy nie zamierzała, aby TB-303 był używany do tworzenia muzyki tanecznej. Został pierwotnie stworzony jako narzędzie pomocnicze dla gitarzystów. Wymyślili sobie, że ludzie mogliby programować na nich linię basową, do której posiadaliby możliwość jam'owania. Niestety pojawiło się zbyt wiele problemów: były odrobinę zbyt skomplikowane do zaprogramowania, nie brzmiały zbyt dobrze jako zamiennik gitary basowej oraz kosztowały w zakupie. Aby uciąć straty, Roland przestał je produkować po sprzedaży 10,000 sztuk. Po paru latach kurzenia się na półkach gitarzystów, dosyć szybko zaczęły trafiać na póki second hand'ów. Te samotne i odrzucone TB-303 czekały na odkrycie ich przez nową generację eksperymentatorów, którzy zaczęli ich używać w sposób, jakiego Roland nie przewidział do tworzenia zupełnie nowych zwariowanych dźwięków. Tak narodził się Acid House.

Pomimo że dostanie w swoje ręce oryginalnego egzemplarza TB-303 nie jest zbyt łatwe, to na pewno ucieszy Cię fakt, iż możesz zmienić swój komputer w jeden z nich, korzystając z możliwości, jakie daje Sonic Pi. Spójrz, odpal Sonic Pi, wrzuć ten kawałek kodu do pustego buforu i naciśnij przycisk Run:

```
use_synth :tb303
play :e1
```
    
Błyskawiczna acid'owa linia basowa! Zabawmy się...

## Chlupnij Basem

Najpierw zbudujmy żywy arpeggiator, aby sprawić, by było jeszcze ciekawiej. W ostatnim tutorialu dowiedzieliśmy się, jak riffy mogą być po prostu pierścieniami nut, przez które tykamy raz po raz, powtarzając całość od nowa, kiedy tylko dobrniemy do końca. Stwórzmy żywą pętlę robiącą właśnie coś takiego:

```
use_synth :tb303
live_loop :squelch do
  n = (ring :e1, :e2, :e3).tick
  play n, release: 0.125, cutoff: 100, res: 0.8, wave: 0
  sleep 0.125
end
```

Przyjrzyjmy się każdej linii.

1. W pierwszej linii za pomocą funkcji `use_synth` ustawiamy, aby domyślnym syntezatorem był `tb303`.
2. W drugiej linii tworzymy żywą pętlę, która po prostu będzie kręciła się w kółko i nazywamy ją `:squelch`.
3. Trzecia linia jest miejscem, gdzie tworzymy nasz riff - pierścień nut (dźwięk E w 1, 2 i 3 oktawie), przez który po prostu cykamy za pomocą polecenia `.tick`. Definiujemy `n` reprezentującą aktualną nutę z naszego riffu. Znak równości mówi, żeby przypisać wartość z prawej strony do nazwy po lewej stronie. W każdym przebiegu pętli będzie to inna wartość. W pierwszym przebiegu `n` będzie ustawione jako dźwięk `:e1`. Przy drugiej rundzie będzie to `:e2`, potem `:e3', a potem znowu `:e1', kręcąc się tak bez końca.
4. Linia czwarta to miejsce, gdzie faktycznie uruchamiamy nasz syntezator `:tb303`. Przekazujemy do niego tutaj kilka interesujących opcji: `release:`, `cutoff:`, `res:` i `wave:`, które omówimy poniżej.
5. Czwarta linia jest naszym miejscem drzemki (polecenie `sleep`) - prosimy żywą pętlę, aby kręciła się co każde '0.125' sekundy, co daje nam 8 razy na sekundę przy domyślnym tempie 60 BPM (z ang. Beats Per Minute - uderzenia na minutę).
6. Linia szósta jest miejscem, gdzie żywa pętla ma swój koniec (`end`). Mówi ona Sonic Pi o miejscu zakończenia żywej pętli.

Podczas gdy wciąż próbujesz połapać się, co jest grane, wpisz powyższy kod i naciśnij przycisk Run. Powinieneś usłyszeć, jak `:tb303` wkracza do działania. A teraz moment, w którym coś się dzieje: zacznijmy kodowanie na żywo.

Podczas gdy pętla wciąż się kręci, zmień opcję `cutoff:` na wartość `110`. Teraz ponownie naciśnij przycisk Run. Powinieneś usłyszeć, że dźwięk stał się trochę ostrzejszy i bardziej chlupotliwy. Wprowadź `120` i naciśnij przycisk Run. Teraz `130`. Posłuchaj jak wyższe wartości odcięcia (ang. cutoff) powodują, że dźwięk staje się bardziej przeszywający i intensywny. Na koniec, kiedy poczujesz, że masz na to ochotę, obniż wartość do `80`. Następnie możesz to powtarzać tyle razy, ile tylko chcesz. Nie przejmuj się, nadal będę tutaj...

Kolejną opcja wartą tego, by się nią pobawić, jest `res:`. Kontroluje ona poziom rezonansu filtra. Wysoki rezonans jest charakterystyczny dla acid'owych dźwięków basu. Wartość naszego parametru `res:` wynosi `0.8`. Spróbuj podkręcić go do poziomu `0.85`, potem do `0.9` i na końcu na `0.95`. Możesz zauważyć, że odcięcie (cutoff) na poziomie `110` lub wyższym będzie powodować, że łatwiej uda Ci się dostrzec różnicę dźwięku. Na koniec zaszalej i spróbuj wpisać wartość `0.999`, aby uzyskać szalone brzmienia. Przy takim wysokim ustawieniu opcji `res:` filtr odcięcia (cutoff) będziecie rezonował tak mocno, że zacznie sam tworzyć dźwięk!

Na koniec, aby uzyskać duży wpływ na tembr, spróbuj zmienić opcję `wave:` na `1`. W ten sposób wybieramy źródło oscylatora. Domyślna wartość to `0` i daje w efekcie falę piłokształtną, wartość `1` daje falę pulsującą, a `2` - falę trójkątną.

Naturalnie spróbuj różnych riff'ów, zmieniając nuty w pierścieniu lub nawet wybierając nuty ze skal lub akordów. Zabaw się z Twoim pierwszym acid'owym syntezatorem basu.

## Dekonstrukcja TB-303

Konstrukcja oryginalnego TB-303 jest w zasadzie bardzo prosta. Jak możesz zauważyć na poniższym diagramie, składa się on tylko z 4-ech głównych części.

![TB-303 Design](../../../etc/doc/images/tutorial/articles/A.05-acid-bass/tb303-design.png)

Pierwszy to fala oscylatora - podstawowe składniki dźwięku. W tym przypadku mamy do czynienia z falą kwadratową. Następnie mamy obwiednię amplitudy oscylatora, która kontroluje poziom głośności fali kwadratowej na przestrzeni czasu. Można się w Sonic Pi do nich dobrać za pomocą opcji `attack:`, `decay:`, `sustain:` i `release:` wraz z odpowiednikami odpowiadającymi za ich poziom. Aby uzyskać więcej informacji, przeczytaj sekcję 2.4 zatytułowaną 'Czas trwania obwiedni dźwięku', która znajduje się w samouczku wbudowanym w aplikację. Następnie przepuszczamy naszą falę kwadratową opakowaną w obwiednię dźwięku przez filtr rezonansowy niskich tonów. Powoduje to odcięcie wyższych częstotliwości, a jednocześnie dodaje ten fajny efekt rezonansu. To jest moment, w którym zaczyna się robić ciekawie. Wartość odcięcia (cutoff) tego filtra jest również kontrolowana przez jego własną obwiednię! Oznacza to, że mamy niesamowitą kontrolę nad tembrem dźwięku poprzez bawienie się obiema obwiedniami. Przyjrzyjmy się temu bliżej:

```
use_synth :tb303
with_fx :reverb, room: 1 do
  live_loop :space_scanner do
    play :e1, cutoff: 100, release: 7, attack: 1, cutoff_attack: 4, cutoff_release: 4
    sleep 8
  end
end
```
    
W syntezatorze `:tb303` dla każdej standardowej opcji obwiedni dźwięku istnieje odpowiednia opcja odcięcia `cutoff_`. Aby zmienić poziom odcięcia dla fazy ataku, możemy użyć opcji `cutoff_attack:`. Skopiuj kod powyżej do pustego buforu i naciśnij przycisk Run. Usłyszysz zwariowane dźwięki jodłujące w jedną i w drugą stronę. Teraz zacznij się bawić. Spróbuj zmienić czas trwania `cutoff_attack:` na 1, a następnie na `0.5`. Teraz z kolei spróbuj wartości `8`.

Zauważ, że dla uzyskania ekstra atmosfery przepuściłem wszystkie dźwięki przez efekt `:reverb` - spróbuj innych efektów i przekonaj się, który z nich się tutaj sprawdzi!

## Połączenie tego wszystkiego w jedną całość

I na koniec kawałek, który skomponowałem, wykorzystując wszystkie pomysły z tego samouczka. Skopiuj go do pustego buforu, posłuchaj przez chwilę, po czym zacznij kodować na żywo, wprowadzając Twoje własne zmiany. Spróbuj zobaczyć, jakie zwariowane dźwięki możesz uzyskać! Do zobaczenia w kolejnym odcinku...

```
use_synth :tb303
use_debug false
 
with_fx :reverb, room: 0.8 do
  live_loop :space_scanner do
    with_fx :slicer, phase: 0.25, amp: 1.5 do
      co = (line 70, 130, steps: 8).tick
      play :e1, cutoff: co, release: 7, attack: 1, cutoff_attack: 4, cutoff_release: 4
      sleep 8
    end
  end
 
  live_loop :squelch do
    use_random_seed 3000
    16.times do
      n = (ring :e1, :e2, :e3).tick
      play n, release: 0.125, cutoff: rrand(70, 130), res: 0.9, wave: 1, amp: 0.8
      sleep 0.125
    end
  end
end
```
