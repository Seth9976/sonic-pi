A.1 Wskazówki dla Sonic Pi

# Pięć najważniejszych wskazówek

## 1. Błędów nie ma

Najważniejszą lekcją, jaką powinieneś wynieść z Sonic Pi, jest taka, że tak naprawdę błędów nie ma. Najlepsza metoda nauki to ciągłe próbowanie, próbowanie i próbowanie. Próbuj wielu różnych pomysłów oraz nie przejmuj się tym, czy Twój kod brzmi dobrze czy źle - zacznij eksperymenty z możliwie największą liczbą różnych syntezatorów, nut, efektów (FX) i opcji. Odkryjesz ogrom rzeczy, które spowodują, że będziesz się śmiać. Niektóre będą brzmieć strasznie, a inne będą prawdziwymi perełkami brzmiącymi niesamowicie dobrze. Wystarczy, że pominiesz rzeczy, które Ci się nie podobają i zostawisz tylko te wpadające Ci w ucho. Im więcej 'błędów' pozwolisz sobie popełnić, tym szybciej się będziesz uczyć i odkryjesz kod, który oddaje Twoje indywidualne brzmienie.


## 2. Używaj efektów (FX)

Więc mówisz, że właśnie opanowałeś podstawy Sonic Pi polegające na tworzeniu dźwięków z wykorzystaniem poleceń `sample` i `play`? Co dalej? Czy wiedziałeś, że Sonic Pi wspiera ponad 27 efektów studyjnych (FX), które pozwalają na zmianę brzmienia Twojego kodu? Są one niczym ekstrawaganckie filtry obrazów, jakie możesz znaleźć w programach graficznych, z tym że zamiast rozmazywania lub sprawiania, iż coś staje się czarno białe, możesz dodać rzeczy takie jak pogłos (ang. reverb), zniekształcenie (ang. distortion) lub echo do Twoich dźwięków. Pomyśl o tym jak o kablu, który łączy Twoją gitarę z pedałami do efektów, które sam wybrałeś, a następnie prowadzi do wzmacniacza. Na szczęście Sonic Pi sprawia, że używanie efektów jest naprawdę proste i nie wymaga żadnych kabli! Jedyne, co musisz zrobić, to wybrać sekcję Twojego kodu, która powinna być objęta efektem i otoczyć ją kodem FX. Spójrzmy na przykład - powiedzmy, że masz następujący kawałek kodu:

```
sample :loop_garzul
16.times do
  sample :bd_haus
  sleep 0.5
end
```

Jeśli chcesz nałożyć efekt na sampel `:loop_garzul`, wystarczy, że wsadzisz go w blok `with_fx` w taki sposób:

```
with_fx :flanger do
  sample :loop_garzul
end
16.times do
  sample :bd_haus
  sleep 0.5
end
```

A teraz, jeśli chcesz dodać efekt do bębna, to jego też po prostu otocz poleceniem `with_fx`:

```
with_fx :flanger do
  sample :loop_garzul
end
with_fx :echo do
  16.times do
    sample :bd_haus
    sleep 0.5
  end
end
```

Zapamiętaj - możesz otoczyć *dowolny* kawałek kodu poleceniem `with_fx` i każdy z tworzonych dźwięków będzie przepuszczony przez ten efekt (FX).


## 3. Używaj parametrów w Twoich syntezatorach

Jeśli naprawdę chcesz odkryć Twoje własne brzmienie w kodzie, to dość szybko będziesz chciał dowiedzieć się, w jaki sposób zmieniać i kontrolować syntezatory oraz efekty. Na przykład możesz chcieć zmienić czas trwania jakiejś nuty, dodać więcej pogłosu (reverb) lub zmienić odstęp pomiędzy echo. Na szczęście Sonic Pi daje Ci niesamowity poziom kontroli nad tymi aspektami za pomocą rzeczy, które nazywają się parametrami opcjonalnymi, w skrócie opcjami. Rzućmy okiem. Skopiuj poniższy kawałek kodu do buforu i naciśnij przycisk "Run":

```
sample :guit_em9
```

Ooo, przyjemny dźwięk gitary! A teraz spróbujmy się tym pobawić. Co powiesz na zmianę tempa?

```
sample :guit_em9, rate: 0.5
```

Hej, czym jest ten kawałek `rate: 0.5`, który przed chwilą dodałem na końcu? Nazywamy to opcją. Wszystkie syntezatory i efekty w Sonic Pi je wspierają i jest ich bardzo dużo do zabawy. Są również dostępne dla efektów. Spróbuj tego:

```
with_fx :flanger, feedback: 0.6 do
  sample :guit_em9
end
```

A teraz spróbuj zwiększyć wartość opcji feedback do 1, aby usłyszeć trochę zwariowanych dźwięków! Zajrzyj do dokumentacji, znajdziesz tam wiele dokładnie opisanych szczegółów, które dotyczą wszystkich dostępnych dla Ciebie opcji.


## 5. Koduj na żywo

Najlepszym metodą na szybkie eksperymenty i naukę Sonic Pi jest kodowanie na żywo. Pozwala Ci to na uruchomienie jakiegoś kawałka kodu, a następnie na sukcesywne zmienianie i ulepszanie go, gdy ten cały czas gra. Przypuśćmy, że nie wiesz, jak działa opcja `cutoff` na wybrany przez Ciebie sampel - wystarczy, że zaczniesz się nim bawić. Spróbujmy tego! Skopiuj poniższy kawałek kodu do jednego z Twoich buforów:

```
live_loop :experiment do
  sample :loop_amen, cutoff: 70
  sleep 1.75
end
```

Teraz naciśnij przycisk "Run", a usłyszysz lekko przytłumiony breakbit. Następnie mień wartość opcji `cutoff:` na 80 i ponownie naciśnij "Run". Słyszysz różnicę? Spróbuj tego samego dla kolejnych wartości: `90`, `100`, `110`...

Gdy raz spróbujesz polecenia `live_loop`, to już nigdy nie będziesz chciał wrócić. Zawsze, kiedy koduję na żywo na jakiejś imprezie, polegam na poleceniu `live_loop` tak samo, jak perkusista polega na swoich pałeczkach. Jeśli chcesz dowiedzieć się więcej o kodowaniu na żywo, zajrzyj do rozdziału nr 9 samouczka, który jest wbudowany w Sonic Pi.

## 5. Surfuj po losowych strumieniach

Jedna z rzeczy, którą uwielbiam robić, to oszukiwanie poprzez wykorzystywanie Sonic Pi, aby komponował melodie za mnie. Bardzo dobrym sposobem jest wykorzystanie do tego celu randomizacji. Może to brzmieć odrobinę skomplikowanie, ale tak naprawdę nie jest. Przyjrzyjmy się temu bliżej - skopiuj poniższy kod do jakiegoś pustego buforu:

```
live_loop :rand_surfer do
  use_synth :dsaw
  notes = (scale :e2, :minor_pentatonic, num_octaves: 2)
  16.times do
    play notes.choose, release: 0.1, cutoff: rrand(70, 120)
    sleep 0.125
  end
end
```

Teraz, kiedy włączysz ten kawałek, usłyszysz stały strumień losowych nut z gamy (skali) pentatonicznej e-moll zagranej przez syntezator `:dsaw`. "Czekaj, czekaj! Przecież to nie jest melodia" Usłyszałem jak krzyczysz! Spokojnie, to pierwsza część magicznej ścieżki. Możemy powiedzieć Sonic Pi, aby zmieniał losowy strumień dźwięków po każdym przebiegu pętli. To jest trochę podobne do sytuacji, w której przenieślibyśmy się w czasie i przestrzeni za pomocą TARDIS'a razem z Doktorem Who. Spróbuj dodać linijkę `use_random_seed 1` w pętli `live_loop`:

```
live_loop :rand_surfer do
  use_random_seed 1
  use_synth :dsaw
  notes = (scale :e2, :minor_pentatonic, num_octaves: 2)
  16.times do
    play notes.choose, release: 0.1, cutoff: rrand(70, 120)
    sleep 0.125
  end
end
```

Teraz, przy każdej kolejnej iteracji pętli, losowy strumień jest resetowany. Oznacza to, że za każdym razem losowane są te same nuty. Bułka z masłem! Melodia błyskawiczna. A teraz czas na coś naprawdę ekstra. Zmień wartość przy poleceniu `seed` na inną liczbę. Powiedzmy `4923`. Łał! Kolejna melodia! Wystarczy więc zmienić jedną liczbę, aby móc poznawać tyle kombinacji melodii, ile tylko sobie wyobrazisz. I to jest właśnie magia kodu!
