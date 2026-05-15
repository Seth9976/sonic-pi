A.13 Kodowanie Probabilistycznego Sekwencera

# Kodowanie Probabilistycznego Sekwencera

W poprzednim odcinku tej serii Sonic Pi odkrywaliśmy niesamowite możliwości randomizacji, aby móc zaprezentować różnorodność, zaskoczenie i zmianę w naszych utworach kodowanych na żywo oraz koncertach. Przykładowo wybieraliśmy losowe nuty z gamy do utworzenia nieskończonej melodii. Dzisiaj nauczymy się nowej techniki, która wykorzystuje randomizację dla rytmu - probabilistyczne uderzenia!

## Prawdopodobieństwo

Zanim będziemy mogli zacząć tworzenie nowych beat'ów i syntezowanych rytmów, musimy zrobić szybką wycieczkę w krainę podstaw prawdopodobieństwa. Może to brzmieć odrobinę strasznie i skomplikowanie, ale tak naprawdę to jest to proste jak rzut kostką do gry - serio! Kiedy weźmiesz do ręki standardową kostkę do gry o 6 oczkach i rzucisz ją, to co tak naprawdę się dzieje? Dobrze, najpierw wyrzucisz 1, 2, 3, 4, 5 lub 6 oczek, z czego każda opcja będzie miała takie same szanse powodzenia. Tak naprawdę jeśli wiemy, że mamy do czynienia z kostką o 6 oczkach, to przeciętnie (jeśli będziesz rzucał bardzo wiele razy) powinieneś wyrzucić 1 przynajmniej raz na każde 6 rzutów. Oznacza to, że masz 1/6 szansy na to, aby wyrzucić 1. W Sonic Pi możemy naśladować takie rzuty kostką za pomocą funkcji `dice`. Spróbujmy rzucić jedną kostką 8 razy:

```
8.times do
  puts dice
  sleep 1
end
```

Zauważ, jak w logu są wyświetlane wartości od 1 do 6 - dokładnie tak samo, jak byśmy rzucali prawdziwą kostką do gry.

## Losowe Beat'y

A teraz wyobraź sobie, że masz bęben i za każdym, gdy chcesz w niego uderzyć, rzucasz kostką. Jeśli wyrzuciłeś 1 oczko - uderzasz, jeśli wyrzuciłeś cokolwiek innego, nie uderzasz. Masz teraz probabilistyczną maszynę perkusyjną, która pracuje z prawdopodobieństwem 1 na 6! Posłuchajmy, jak to brzmi:

```
live_loop :random_beat do
  sample :drum_snare_hard if dice == 1
  sleep 0.125
end
```


Spróbujmy szybko przyjrzeć się każdej linii, aby upewnić się, że wszystko jest jasne i zrozumiałe. Najpierw tworzymy nową żywą pętlę `live_loop` o nazwie `:random_beat`, która będzie w kółko powtarzać 2 linie pomiędzy poleceniami `do` i `end`. Pierwsza z tych linii to wywołanie polecenia `sample`, które spowoduje, iż zostanie odtworzony nagrany wcześniej dźwięk (w tym wypadku będzie to `:drum_snare_hard`). Zauważ jednak, że linia ta ma na końcu specjalną klauzulę `if`. Oznacza ona, iż dany wiersz zostanie wykonany tylko wtedy, gdy warunek logiczny po prawej stronie jest prawdą i zwróci wartość `true`. Nasz warunek w tym wypadku to `dice == 1`. Powoduje to zawołanie naszej funkcji `dice`, która, jak już widzieliśmy, zwraca wartości pomiędzy 1 a 6. Następnie używamy operatora `==`, aby sprawdzić, czy wylosowana wartość to `1`. Jeśli tak, to nasze wyrażenie logiczne zwraca wartość `true` i usłyszymy dźwięk naszego werbla (z ang. snare drum). Jeśli jednak zwrócona wartość to nie `1`, wtedy nasze wyrażenie logiczne zwróci wartość `false` i nasz werbel zostanie ominięty. Druga linia po prostu sprawia, że czekamy `0.125` sekundy, zanim ponownie wykonamy rzut kostką.

## Zmienianie Prawdopodobieństwa

Ci z Was, którzy mieli okazję grać w gry fabularne (RPG - z ang. role-playing game), będą zapewne zaznajomieni z wieloma kostkami o dziwnych kształtach, które mają różną ilość oczek. Na przykład istnieje kostka o kształcie czworościanu, która posiada 4 ścianki, a nawet kostka o 20 ściankach w kształcie dwudziestościanu. Liczba ścian kostki zmienia szansę lub prawdopodobieństwo wyrzucenia 1 oczka. Im mniej ścianek, tym większa szansa na to, że wyrzucisz 1 i analogicznie im więcej ścianek, tym prawdopodobieństwo jest mniejsze. Przykładowo, korzystając z kostki o 4 oczkach, mamy szansę jeden do czterech na wyrzucenie liczby 1, a korzystając z kostki z 20 oczkami, jest tylko jedna dwudziesta szansy. Na szczęście Sonic Pi posiada bardzo wygodną funkcję `one_in`, która została stworzona właśnie do tego celu. Zagrajmy:

```
live_loop :different_probabilities do
  sample :drum_snare_hard if one_in(6)
  sleep 0.125
end
```

Uruchom powyższą żywą pętlę, a usłyszysz znany Ci już rytm o losowym brzmieniu. Nie zatrzymuj jednak tego kodu. Zamiast tego, w trakcie działania, zmień wartość `6` na inną, taką na przykład jak `2` lub `20` i naciśnij ponownie przycisk `Run`. Zauważ, że mniejsze liczby oznaczają, iż werbel będzie brzmiał częściej, a wyższe liczby spowodują, że będziemy go słyszeć rzadziej. Właśnie tworzysz muzykę, korzystając z mechanizmu prawdopodobieństwa!

## Łączenie Prawdopodobieństwa

Bardzo ciekawe efekty można uzyskać wtedy, gdy połączymy jednocześnie uruchamianie wielu sampli z różnym prawdopodobieństwem. Na przykład:

```
live_loop :multi_beat do
  sample :elec_hi_snare if one_in(6)
  sample :drum_cymbal_closed if one_in(2)
  sample :drum_cymbal_pedal if one_in(3)
  sample :bd_haus if one_in(4)
  sleep 0.125
end
```

A teraz uruchom powyższy kod i zacznij zmieniać prawdopodobieństwa, aby zmienić rytm. Spróbuj również zmieniać sample, żeby utworzyć zupełnie nowe brzmienie. Na przykład spróbuj zmienić sampel `:drum_cymbal_closed` na `:bass_hit_c` dla uzyskania ekstra bass'u!


## Powtarzalne Rytmy

Następnie możemy użyć naszej starej, dobrej znajomej funkcji `use_random_seed`, aby zresetować losowy strumień po 8 iteracjach, żeby utworzyć regularny beat. Wpisz następujący kod dla usłyszenia bardziej regularnego i powtarzalnego rytmu. Gdy już usłyszysz beat, spróbuj zmienić wartość ziarna z `1000` na inną liczbę. Zauważ, jak różne liczby będą generowały różne beaty.

```
live_loop :multi_beat do
  use_random_seed 1000
  8.times do
    sample :elec_hi_snare if one_in(6)
    sample :drum_cymbal_closed if one_in(2)
    sample :drum_cymbal_pedal if one_in(3)
    sample :bd_haus if one_in(4)
    sleep 0.125
  end
end
```

Kiedy pracuję z tego typu strukturami, staram się zazwyczaj zapamiętać, które ciągi dźwięków brzmią fajnie i staram się je zapisywać. W ten sposób mogę bardzo łatwo odtwarzać moje rytmy w przyszłych wprawkach czy wystąpieniach na żywo.

## Połączenie tego wszystkiego w jedną całość

Na koniec możemy wrzucić tu trochę losowego basu, aby nadać mu fajną melodyjny charakter. Zauważ, że tutaj również możemy użyć naszej świeżo odkrytej metody probabilistycznego sekwencjonowania dla naszych syntezatorów - dokładnie w taki sam sposób, jak zrobiliśmy to w przypadku sampli. Nie zostawiaj tego jednak samemu sobie - spróbuj zmieniać liczby i zacznij tworzyć Twoje własne numery, korzystając z mocy probabilistyki!

```
live_loop :multi_beat do
  use_random_seed 2000
  8.times do
    c = rrand(70, 130)
    n = (scale :e1, :minor_pentatonic).take(3).choose
    synth :tb303, note: n, release: 0.1, cutoff: c if rand < 0.9
    sample :elec_hi_snare if one_in(6)
    sample :drum_cymbal_closed if one_in(2)
    sample :drum_cymbal_pedal if one_in(3)
    sample :bd_haus, amp: 1.5 if one_in(4)
    sleep 0.125
  end
end
```
