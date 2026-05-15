A.11 Tik Tak

# Śledzenie Rytmu

W poprzednim numerze tego poradnika mieliśmy okazję zgłębiać bardzo techniczne tematy związane z systemem randomizacji, który jest fundamentalną częścią Sonic Pi. Dowiedzieliśmy się, w jaki sposób możemy użyć go, aby wynieść możliwość dynamicznej kontroli nad naszym kodem na zupełnie nowy poziom. W tym miesiącu zamierzamy kontynuować naszą techniczną wycieczkę i zwrócimy naszą uwagę ku unikalnemu systemowi tykania dostępnego w Sonic Pi. Pod koniec tego artykułu będziesz tykał po swojemu poprzez rytmy i riffy, aby stać się DJ'em kodującym na żywo.

## Licznik Uderzeń

Podczas tworzenia muzyki często mamy ochotę na zrobienie czegoś innego, w zależności od którego uderzenia to jest. Sonic Pi posiada specjalny system liczenia uderzeń zwany `tick`. Daje on precyzyjną kontrolę podczas występowania uderzenia, a także i obsługuje wiele uderzeń mających własne tempa.

Zabawmy się - by przyspieszyć uderzanie, musimy tylko wezwać `tick`. Otwórz czysty bufor, by następnie wpisać poniższy fragment kodu i nacisnąć Run:

```
puts tick #=> 0
```

Spowoduje to zwrócenie aktualnego bitu: `0`. Zauważ, że nawet gdy naciśniesz kilkukrotnie przycisk Run, to i tak zawsze zostanie zwrócona wartość `0`. Dzieje się tak, ponieważ każde uruchomienie spowoduje, że bit zacznie być liczony od 0. Jednakże tak długo, jak nasza muzyka jest uruchomiona, możemy przechodzić do kolejnych uderzeń tak wiele razy, jak tylko chcemy:

```
puts tick #=> 0
puts tick #=> 1
puts tick #=> 2
```

W każdym miejscu, w którym zobaczysz symbol `#=>` na końcu danej linii kodu, oznacza to, że linia ta spowoduje zalogowanie tekstu po prawej stronie okna. Na przykład `puts foo #=> 0` oznacza, iż kod `puts foo` wyświetli `0` w logach w danym punkcie programu.

## Sprawdzanie Bitu

Widzieliśmy, że `tick`anie robi dwie rzeczy. Powiększa ono (dodaje jedno) i powraca do obecnego uderzenia. Czasami chcemy tylko spojrzeć na obecne uderzenia bez zwiększania ich ilości. Do tego zadania wykorzystamy polecenie `look`:

```
puts tick #=> 0
puts tick #=> 1
puts look #=> 1
puts look #=> 1
```

W tym kawałku kodu cykniemy nasz bit dwa razy, a następnie dwa razy uruchomimy polecenie `look`. W logu zobaczymy następujące wartości: `0`, `1`, `1`, `1`. Pierwsze dwa tyknięcia (wywołania polecenia `tick`) zwracają `0`, potem `1` tak jak tego oczekujemy, a kolejne dwa polecenia `look` zwracają po prostu ostatnią wartość bitu dwa razy, co w tym przypadku oznacza `1`.


## Pierścienie

Teraz możemy więc zwiększać bit z wykorzystaniem polecenia `tick` i sprawdzać go z wykorzystaniem polecenia `look`? Co teraz? Potrzebujemy czegoś, przez co moglibyśmy tykać. Sonic Pi wykorzystuje pierścienie do reprezentacji riffów, melodii i rytmów, natomiast system tykania został zaprojektowany z myślą o ścisłej współpracy z nimi. W rzeczywistości pierścienie posiadają swoją własną kropkową wersję polecenia `tick`, która robi dwie rzeczy. Po pierwsze, zachowuje się tak jak normalne tykanie i powoduje podniesienie wartości aktualnego bitu. Po drugie, przeszukuje wartości pierścienia, wykorzystując dane tyknięcie bitu jako indeks. Spójrzmy na to:

```
puts (ring :a, :b, :c).tick #=> :a
```

Polecenie `.tick` to specjalna kropkowa wersja polecenia `tick`, która zwraca w tym przypadku pierwszą wartość z pierścienia - `:a`. Możemy dostać się do każdej z wartości w pierścieniu przez kilkukrotne wywołanie polecenia `.tick`:

```
puts (ring :a, :b, :c).tick #=> :a
puts (ring :a, :b, :c).tick #=> :b
puts (ring :a, :b, :c).tick #=> :c
puts (ring :a, :b, :c).tick #=> :a
puts look                   #=> 3
```

Zwróć uwagę na log, a zauważysz, że zostały wypisane kolejno `:a`, `:b`, `:c`, a potem znowu `:a`. Zauważ, iż polecenie `look` zwraca wartość `3`. Kolejne uruchomienia polecenia `.tick` zachowują się dokładnie tak samo jak normalne standardowe wywołania polecenia `tick` - zwiększają aktualną wartość dla lokalnego wskaźnika bitu.


## Arpeggiator Żywej Pętli

Prawdziwą moc zobaczysz, gdy połączysz razem polecenie `tick` z pierścieniami (`ring`) i żywymi pętlami (`live_loop`). Gdy je połączymy, to mamy wszystkie narzędzia, które są potrzebne, aby zbudować i zrozumieć prosty arpeggiator. Potrzebujemy tylko czterech rzeczy:

1. Pierścień zwracający nuty, przez które będziemy iterować.
2. Środek, który pozwoli nam na inkrementację i dostęp do aktualnego licznika bitów.
3. Zdolność do zagrania nuty na podstawie aktualnego bitu.
4. Struktura pętli zachowująca powtarzanie arpegiatora.

Te pojęcia można znaleźć w poniższym kodzie:

```
notes = (ring 57, 62, 55, 59, 64)
live_loop :arp do
  use_synth :dpulse
  play notes.tick, release: 0.2
  sleep 0.125
end
```

Przyjrzyjmy się uważnie każdej z tych linii. Najpierw definiujemy pierścień z naszymi nutami, które będziemy grać w kółko. Następnie tworzymy żywą pętlę `live_loop` o nazwie `:arp` kręcącą się dla nas w kółko. Przy każdym kolejnym przebiegu naszej żywej pętli jako aktualny syntezator ustawiamy `:dpulse`, a następnie gramy kolejną nutę z naszego pierścienia, korzystając z polecenia `.tick`. Pamiętaj, że spowoduje to, iż nasz licznik bitów będzie się zwiększał i każde jego kolejne użycie będzie korzystało z ostatniego bitu jako indeksu do wyciągania nut z naszego pierścienia. Na koniec chcemy poczekać przez jedną ósmą uderzenia zanim rozpoczniemy kolejny przebieg żywej pętli.

## Wielokrotne Równoczesne Uderzenia

Bardzo ważną rzeczą, którą warto wiedzieć, jest to, iż polecenia `tick`i są lokalne dla danej pętli `live_loop`. Oznacza to, iż każda żywa pętla `live_loop` posiada własny niezależny licznik uderzeń. Takie podejście daje o wiele więcej możliwości niż posiadanie jednego globalnego metronomu i bitu. Rzućmy okiem na to, jak to działa w praktyce:

```
notes = (ring 57, 62, 55, 59, 64)
with_fx :reverb do
  live_loop :arp do
    use_synth :dpulse
    play notes.tick + 12, release: 0.1
    sleep 0.125
  end
end
live_loop :arp2 do
  use_synth :dsaw
  play notes.tick - 12, release: 0.2
  sleep 0.75
end
```

## Kolidujące Uderzenia

Duża przyczyna zamieszania z systemem cykania Sonic Pi jest wtedy, gdy ludzie chcą użyć go na wielu pierścieniach w tym samym `live_loop`:

```
use_bpm 300
use_synth :blade
live_loop :foo do
  play (ring :e1, :e2, :e3).tick
  play (scale :e3, :minor_pentatonic).tick
  sleep 1
end
```

Nawet jeśli każda żywa pętla `live_loop` posiada swój własny niezależny licznik bitów, to wywołujemy polecenie `.tick` dwa razy w ramach tej samej żywej pętli. Ta informacja oznacza, że bit zostanie zwiększony dwa razy przy każdej rundzie. Może to spowodować pewne interesujące polirytmie, ale zazwyczaj nie jest to coś, czego byś oczekiwał. Istnieją dwa rozwiązania tego problemu. Pierwsza opcja to manualne uruchomianie polecenia `tick` na początku każdej żywej pętli `live_loop`, a następnie korzystanie z polecenia `.look`, aby odwołać się do aktualnego uderzenia w każdym przebiegu żywej pętli. Drugie rozwiązanie to przekazanie unikalnej nazwy do każdego wywołania polecenia `.tick`, na przykład `.tick(:foo)`. Sonic Pi stworzy wtedy i będzie śledził oddzielny licznik bitów dla każdej użytej przez Ciebie z nazw. Tym sposobem masz możliwość pracy z tyloma licznikami uderzeń, z iloma będziesz tylko chciał. Aby dowiedzieć się więcej, zwróć uwagę na sekcję poświęconą nazwanym cyknięciom, która jest dostępna w puncie 9.4 samouczka wbudowanego w Sonic Pi.

## Połączenie tego wszystkiego w jedną całość

Doprowadźmy tę całą wiedzę na temat `tick`ania, `ring`ów i `live_loop`ów razem do ostatecznego przykładu zabawy. Jak przywykliśmy już do tego, nie traktuj tego jako gotowy kawałek - rozpocznij zmianę wielu rzeczy i baw się z nimi. Za pomocą swojej wyobraźni wyczaruj coś, co Cię zachwyci. Do zobaczenia następnym razem...

```
use_bpm 240
notes = (scale :e3, :minor_pentatonic).shuffle
live_loop :foo do
  use_synth :blade
  with_fx :reverb, reps: 8, room: 1 do
    tick
    co = (line 70, 130, steps: 32).tick(:cutoff)
    play (octs :e3, 3).look, cutoff: co, amp: 2
    play notes.look, amp: 4
    sleep 1
  end
end
live_loop :bar do
  tick
  sample :bd_ada if (spread 1, 4).look
  use_synth :tb303
  co = (line 70, 130, steps: 16).look
  r = (line 0.1, 0.5, steps: 64).mirror.look
  play notes.look, release: r, cutoff: co
  sleep 0.5
end
```
