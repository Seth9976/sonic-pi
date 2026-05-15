A.19 Design Dźwięku - Synteza Subtraktywna

# Synteza Subtraktywna

Oto drugi z serii artykułów poświęconych designowi dźwięku w Sonic Pi! W poprzednim miesiącu rzuciliśmy okiem na syntezę addytywną, dzięki której odkryliśmy, jak łatwe jest granie wielu dźwięków w tym samym czasie do stworzenia różnorakich kombinacji. Na przykład mogliśmy połączyć różnie brzmiące syntezatory, a nawet te same z nich w różnych wysokościach dźwięku, by zbudować nowy, bardziej złożony dźwięk z prostych składników. W tym miesiącu zajmiemy się świeżą techniką zwaną zwyczajnie *syntezą subtraktywną*, która bierze złożone dźwięki oraz usuwa niektóre części z nich do utworzenia czegoś całkowicie nowego. Ten sposób jest powiązany z dźwiękiem analogowych syntezatorów z lat 60. oraz 70., ale również z wczesnym renesansem, idąc poprzez popularnych standardy takie jak Eurorack.

Pomimo iż to brzmi jak szczególnie skomplikowana i zaawansowana technika, Sonic Pi sprawia, że jest to zaskakująco proste oraz łatwe - zanurkujmy więc!

## Złożone Źródło Sygnału

Dźwięk, który ma pracować dobrze z syntezą subtraktywną, zazwyczaj musi być dość bogaty i ciekawy. To nie oznacza jednak, że potrzebujemy czegoś ogromnie złożonego - w rzeczywistości standardowe fale `:square` lub `:saw` w zupełności wystarczą:

```
synth :saw, note: :e2, release: 4
```

Zauważ to, iż dźwięk jest całkiem intrygujący oraz zawiera wiele różnych częstotliwości powyżej `:e2` (drugi klawisz E na fortepianie), które dodaje się, by utworzyć barwę dźwięku. Jeżeli to na razie nie ma dla Ciebie większego sensu, spróbuj porównać go z `:beep`:

```
synth :beep, note: :e2, release: 4
```

Ze względu na to, iż syntezator `:beep` jest po prostu falą sinusoidalną, usłyszysz o wiele czystszy ton tylko na `:e2` oraz żaden z tych chrupiących/brzęczących dźwięków, z którymi miałeś do czynienia w `:saw`. To jest brzęczenie i odmiana z czystej fali sinusoidalnej, gdzie możemy grać, kiedy używamy syntezy subtraktywnej.

## Filtry

Gdy mamy już nasze pierwotne źródło sygnału, następnym krokiem będzie przejście przez filtr pewnego typu, który zmodyfikuje dźwięk za pomocą usunięcia lub zredukowania jakiejś części. Jeden z najczęstszych filtrów używanych w syntezie subtraktywnej nazywany jest filtrem dolnoprzepustowym. Pozwala on przejść wszystkim niższym częściom dźwięku, lecz redukując przy tym także wyższe. Sonic Pi posiada potężny, ale prosty w obsłudze system FX, który zawiera filtr dolnoprzepustowy o nazwie `:lpf`. Zabawmy się:

```
with_fx :lpf, cutoff: 100 do
  synth :saw, note: :e2, release: 4
end
```

Jeśli wsłuchasz się uważnie, usłyszysz, jak parę z tych brzęczących i chrupiących dźwięków zostało usuniętych. W rzeczywistości wszystkie częstotliwości dźwięku powyżej nuty `100` zostały zredukowane lub usunięte i tylko te niższe są w nim obecne. Spróbuj zamienić parametr `cutoff:` do niższych nut, przykładowo `70`, a następnie `50`, by je porównać.

Oczywiście parametr `:lpf` nie jest jedynym filtrem, którego możesz użyć do kontrolowania źródłem sygnału. Kolejnym równie ważnym FX jest filtr górnoprzepustowy określony jako `:hpf`. Robi dokładnie to, co `:lpf`, tylko odwrotnie - pozwala on przejść wszystkim wyższym częściom dźwięku, redukując lub usuwając niższe.

```
with_fx :hpf, cutoff: 90 do
  synth :saw, note: :e2, release: 4
end
```

Zauważ, jak dźwięk brzmi teraz bardziej brzęcząco i chrupiąco, a to wszystko z tego powodu, iż niskie częstotliwości zostały usunięte. Pobaw się z wartością cutoff i zobacz, jak niskie wartości pozwalają istnieć oryginalnym częściom basu źródła sygnału oraz jak wysokie wartości zwiększają brzękliwość oraz ciszę.

## Low Pass Filter

![Varying amounts of low pass filtering](../../../etc/doc/images/tutorial/articles/A.19-subtractive-synthesis/subtractive-synthesis-waveforms.png)

Filtr dolnoprzepustowy jest ważną częścią narzędzi każdej syntezy subtraktywnej - warto zatem zanurzyć się głębiej, by zobaczyć, jak on działa. Ten diagram pokazuje tę samą falę dźwięku (syntezator `:prophet`) z różnymi wartościami filtrowania. U góry, w sekcji A, pokazuje falę dźwiękową bez filtrowania. Zauważ, że fala ta jest bardzo punktowa i zawiera wiele ostrych krawędzi. To właśnie one produkują tę chrupiącą/brzęczącą część dźwięku. Sekcja B pokazuje wszystkie filtry dolnoprzepustowe w akcji - fala jest mniej punktowa i bardziej zaokrąglona niż powyżej. Oznacza to, iż jeśli dźwięk posiada mniejszą częstotliwość, daje on bardziej ładne oraz zaokrąglone linie. Sekcja C zawiera filtr dolnoprzepustowy z dość niską wartością funkcji cutoff - im większe częstotliwości zostały usunięte z sygnału, tym skutkuje to bardziej łagodną, zaokrągloną falą. Na końcu rzuć okiem na wielkość formy fali reprezentującej amplitudę, zmniejszającą się podczas przechodzenia od A do C. Synteza subtraktywna działa poprzez usuwanie części sygnału, które oznaczają, iż ogólna amplituda jest redukowana wraz z zwiększaniem się liczby filtrowania.


## Modulacja Filtra

Do tej pory tworzyliśmy dość statyczne dźwięki. Używając innych słów - nie zmieniał się on podczas całej drogi jej trwania. Często możesz chcieć trochę ruchu w dźwięku, aby nadać barwie trochę życia. Aby to osiągnąć, możesz skorzystać z filtra modulacji - zmieniając jego ustawienia poprzez czas. Na szczęście Sonic Pi oferuje potężne narzędzie służące do manipulowania parametrami FX. Przykładowo możesz ustawić czas ślizgu dla każdego modulowanego parametru, aby zdefiniować, jak długo powinien on zająć z obecnej wartości do docelowej:

```
with_fx :lpf, cutoff: 50 do |fx|
  control fx, cutoff_slide: 3, cutoff: 130
  synth :prophet, note: :e2, sustain: 3.5
end
```

Rzućmy okiem na to, co się tutaj dzieje. Początkowo rozpoczynamy funkcją `:lpf` jako normalną z początkowym `cutoff:` o bardzo niskim parametrze `20`. Jednakże pierwsza linia kończy się także dziwnym `|fx|` na końcu. Jest to opcjonalna część składni `with_fx`, która umożliwia nam na bezpośrednie nazwanie i kontrolowanie syntezatora FX. Linia 2 robi to oraz kontroluje FX, by ustawić parametr `cutoff_slide:` na 4 i nowy obiekt docelowy `cutoff:` na `130`. FX rozpocznie prześlizg `cutoff:` z wartości `50` na `130` przez okres 3 uderzeń. Na końcu wyzwalamy sygnał źródłowy syntezatora, przez co możemy usłyszeć efekt modulowanego filtra dolnoprzepustowego.


## Połączenie tego wszystkiego w jedną całość

Do czynienia mamy z bardzo podstawowym zjadaczem, co staje się możliwe, gdy używasz filtrów do edycji i zmiany źródła dźwięku. Spróbuj zagrać z wieloma innymi wbudowanymi FX w Sonic Pi, by zobaczyć, jak wiele szalonych dźwięków możesz zaprojektować. Jeżeli brzmi on zbyt statycznie, pamiętaj o tym, iż możesz modulować wybrane opcje, by utworzyć trochę ruchu.

Zakończmy, projektując funkcję, która zagra nowy dźwięk utworzony za pomocą syntezy substraktywnej. Sprawdź, czy możesz ogarnąć to, co się tutaj dzieje - a dla zaawansowanych Czytelników Sonic Pi - zobacz, czy możesz sprawdzić, dlaczego owinąłem wszystko w środku połączenia do `at` (proszę wysłać rozwiązania do @samaaron na Twitterze w języku angielskim).

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
