A.18 Design Dźwięku - Synteza Addytywna

# Synteza Addytywna

Witaj w pierwszym artykule krótkiej serii na temat użytkowania Sonic Pi w designie dźwięku. Ruszymy w szybką turę różnych technik, z których możesz skorzystać, aby stworzyć własne, unikalne dźwięki. Pierwszą z nich będzie coś, co nosi nazwę *syntezy addytywnej*. To może brzmieć skomplikowanie, ale jeśli wyjaśnimy po kolei każde słowo, znacznie wręcz wyskoczy samoistnie. Słowo "synteza" oznacza tworzenie muzyki, a "addytywna" - kombinację rzeczy. Addytywna synteza to nic zatem bardziej skomplikowanego jak *łączenie istniejących dźwięków, aby stworzyć z nich nowe*. Ta technika pochodzi z bardzo dawnych czasów, przykładowo organy w średniowieczu miały wiele różnie brzmiących piszczałek, które można było aktywować lub dezaktywować z pauzami. Wyciągając, 'dodawano je do miksu', co powodowało, iż wytworzony dźwięk brzmiał bardziej bogato i złożono. Spójrzmy teraz na to, jak możemy te pauzy wyciągać za pomocą Sonic Pi.


## Proste Kompozycje

Rozpocznijmy więc od najbardziej podstawowego dźwięku, który możesz tutaj znaleźć - prostej czystotonowej fali sinusoidalnej:

```
synth :sine, note: :d3
```

Teraz spójrzmy na brzmienie w połączeniu z falą kwadratową:

```
synth :sine, note: :d3
synth :square, note: :d3
```

Zauważ, jak one się łączą, tworząc całkowicie nowe i bogate dźwięki. Jasne jest dla nas to, iż nie musimy się zatrzymywać na tym etapie - możemy dodać tyle dźwięków, ile tak naprawdę potrzebujemy. Jednak musimy być ostrożni podczas ich łączenia, gdyż to jest tak, jak z łączeniem nowych kolorów - kiedy dodamy ich za dużo, wynikiem będzie brzydki brązowy. Ta sama rzecz dzieje się z dźwiękami - pakowanie za dużej ich liczby do jednego pola spowoduje zamianę w coś niezbyt "zjadliwego".


## Mieszanie

Dodajmy coś, aby dźwięk brzmiał trochę jaśniej. Możemy użyć fali trójkątnej z wyższą oktawą (dla tego wysokiego, jasnego dźwięku), ale tylko grając z amplitudą o wartości `0.4`. To tylko dodaje coś ekstra zamiast całkowicie zmienić znaczenie:

```
synth :sine, note: :d3
synth :square, note: :d3
synth :tri, note: :d4, amp: 0.4
```

Teraz spróbuj stworzyć swoje własne dźwięki poprzez połączenie dwóch lub większej ilości syntezatorów z różnymi oktawami i aplitudami. Także zauważ to, jak możesz się bawić każdymi ustawieniami parametrów syntezatorów, których źródło muzyki jest wcześniej miksowane w większą ilość ich kombinacji.


## Strojenie

Łącząc różne syntezatory, używaliśmy tej samej wysokości lub zmienionej oktawy przez całą naszą drogę. Jak może to zabrzmieć, jeżeli nie będziemy się trzymać tego drugiego parametru, a właśnie wybierzemy troszeczkę wyższą lub niższą nutę? Spróbujmy tego:

```
detune = 0.7
synth :square, note: :e3
synth :square, note: :e3 + detune
```

Jeśli rozstroimy nasze fale kwadratowe przez 0.7 nuty usłyszymy coś, co nie brzmi zbyt dobrze, a mówiąc poprawnie - otrzymamy 'złą' nutę. Jednakże jeśli przybliżmy się do 0, to dźwięk będzie bardziej i bardziej nieczysty - jak wysokość dźwięku dwóch fal zbliżających się do siebie. Wypróbuj tego! Zmień wartość parametru `detune:` z `0.7` na `0.5` i wsłuchaj się w nowy dźwięk. Skorzystaj z `0.2`, `0.1`, `0.05`, `0`. Za każdym razem, gdy zmieniasz wartość, poświęć chwilę na wysłuchanie i zobacz, czy możesz usłyszeć to, jak dźwięk się zmienia. Zauważ, że niskie rozstrojenie (np. `0.1`) wytwarza naprawdę niezły 'gęsty' dźwięk z innymi wysokościami każdego z nich -
 zmieniającymi się w ciekawy, często zaskakujący sposób.

Niektóre z wbudowanych syntezatorów obsługują już opcję rozstrojenia, przez co można z niej korzystać w jednym z nich. Spróbuj zagrać z parametrem `detune:` - `:dsaw`, `:dpulse` oraz `:dtri`.


## Kształtowanie Amplitudy

Kolejnym sposobem na tworzenie naszych dźwięków jest użycie innej obwiedni i ustawień dla każdego wyzwalacza syntezatora. Przykładowo pozwoli Ci to na wymyślenie aspektów perkusyjnej muzyki oraz pierścieni na okres czasu.

```
detune = 0.1
synth :square, note: :e1, release: 2
synth :square, note: :e1 + detune, amp: 2, release: 2
synth :gnoise, release: 2, amp: 1, cutoff: 60
synth :gnoise, release: 0.5, amp: 1, cutoff: 100
synth :noise, release: 0.2, amp: 1, cutoff: 90
```

W powyższym przykładzie zmiksowałem hałaśliwy, perkusyjny element wraz z trwałym dudnieniem w tle. Zostało to osiągnięte poprzez użycie dwóch głośnych syntezatorów: jednego ze środkowymi wartościami odcięcia (`90` i `100` i krótkim czasem wyzwolenia) oraz drugiego z niską wartością odcięcia (co powoduje, że dźwięk jest mniej chrupki, a także i bardziej burczący).

## Połączenie tego wszystkiego w jedną całość

Połączmy wszystkie te techniki, by zobaczyć, jak możemy użyć syntezy addytywnej w celu ponownego stworzenia podstawowego dźwięku dzwonka. Podzieliłem ten przykład na cztery sekcje - pierwszy to 'uderzenie', który jest początkiem części dźwięku, zatem używa krótkiej obwiedni (przykładowo `release:` o wartości około `0.1`). Następna z nich to długa sekcja dzwonienia, gdzie używam czystego dźwięku fali sinusoidalnej. Zauważ to, jak często zwiększam nutę w szerokim zakresie `12` oraz `24`, które są numerami nut w jednej i drugiej oktawie. Połączyłem także w parę niskie fale sinusoidalne, aby otrzymać trochę basu i głębi. W końcu skorzystałem z `define`, by ścisnąć mój kod w funkcję służącą do zagrania melodii. Spróbuj stworzyć swoją własną oraz pobaw się z zawartością `:bell` do chwili, gdy otrzymasz swój własny szalony dźwięk!

```
define :bell do |n|
  # Triangle waves for the 'hit'
  synth :tri, note: n - 12, release: 0.1
  synth :tri, note: n + 0.1, release: 0.1
  synth :tri, note: n - 0.1, release: 0.1
  synth :tri, note: n, release: 0.2
  # Sine waves for the 'ringing'
  synth :sine, note: n + 24, release: 2
  synth :sine, note: n + 24.1, release: 2
  synth :sine, note: n + 24.2, release: 0.5
  synth :sine, note: n + 11.8, release: 2
  synth :sine, note: n, release: 2
  # Low sine waves for the bass
  synth :sine, note: n - 11.8, release: 2
  synth :sine, note: n - 12, release: 2
end
# Play a melody with our new bell!
bell :e3
sleep 1
bell :c2
sleep 1
bell :d3
sleep 1
bell :g2
```
