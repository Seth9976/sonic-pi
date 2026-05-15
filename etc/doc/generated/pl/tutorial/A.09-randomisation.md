A.9 Randomizacja

# Surfowanie Po Losowych Strumieniach

W czwartym odcinku tej serii samouczków rzuciliśmy okiem na losowość podczas kodowania paru ciekawych riffów syntezatorów. Biorąc pod uwagę fakt, iż losowość jest ważną częścią mojego życia jako kodujący DJ, pomyślałem, iż przydatne może być bardziej szczegółowe ich wytłumaczenie. Zdobądź więc swoją szczęśliwą czapkę i zasurfujmy kilka losowych strumieni!

## Nie istnieje żadna losowość

Pierwszą rzeczą, która na początku może Cię naprawdę zaskoczyć podczas zabawy z funkcjami losowości w Sonic Pi, jest fakt, iż nie są one rzeczywiście losowe. Co to więc oznacza? Dobrze, zróbmy więc kilka testów. Na starcie wyobraź sobie liczbę między 0 oraz 1. Zachowaj ją dla siebie i nie mów mi jej - to będzie na razie Twoja tajemnica. Niech zgadnę... czy to jest `0.321567`? Nie? Nonsens, najwidoczniej nie jestem w tym dobry. Daj mi kolejną próbę, lecz teraz zapytajmy Sonic Pi o wybranie numeru tym razem. Uruchom Sonic Pi v2.7+ i poproś o losową liczbę, ale pamiętaj, żeby mi jej nie mówić:

```
print rand
```

Teraz dla odkrycia... czy to było `0.75006103515625`? Tak! Heh, widzę, że jesteś trochę sceptyczny. Może to był szczęśliwy traf i wcale nie jestem w tym zły? Spróbujmy jeszcze raz. Naciśnij ponownie przycisk *Run* i zobaczmy, co otrzymaliśmy... Hę? Ponownie `0.75006103515625`? To najwidoczniej nie może być losowe! Masz rację.

Co się tutaj dzieje? Fantazyjnym słowem nauki komputerowej jest tutaj determinizm. To po prostu oznacza, iż nic nie staje się przypadkowo i wszystko ma swoją wartość. Twoja wersja Sonic Pi przeznaczona jest do zwracania zawsze wartości `0.75006103515625`. To może brzmieć trochę bezużytecznie, jednak zapewniam Cię, iż ma to miano najbardziej potężnej części programu. Jeżeli nad tym spędzisz trochę czasu, nauczysz się polegania na determinizmie natury losowości Sonic Pi jako fundamentu budowania bloków dla Twoich kompozycji i kodowanych na żywo zestawów DJ-ów.

## Losowa Melodia

Kiedy Sonic Pi startuje, to właściwie ładuje do pamięci sekwencję 441,000 wstępnie wygenerowanych losowych wartości. Po wywołaniu funkcji losowej jak `rand` lub `rrand`, strumień wykorzystywany jest do generowania wyników. Dosłownie każde użycie tych funkcji używa danej wartości - dziesiąte użycie funkcji losowania także użyje dziesiątej wartości ze strumienia. Także za każdym razem, kiedy naciśniesz przycisk *Run*, jest on resetowany dla tego uruchomienia. Właśnie dzięki temu mogę przewidzieć wynik dla `rand` i dlaczego ta 'losowa' melodia jest za każdym razem taka sama. Każda wersja Sonic Pi wykorzystuje dokładnie ten sam losowy strumień, co odgrywa ważną rolę podczas udostępniania naszych kawałków innym.

Użyjmy zatem tej wiedzy do wygenerowania losowo powtarzanej melodii:

```
8.times do
 play rrand_i(50, 95)
 sleep 0.125
end
```

Wpisz ten kod do wolnego bufora i wciśnij *Run*. Usłyszysz melodię, która składa się z losowych nut z przedziału między 50 a 95. Po jego zakończeniu ponownie to zrób, aby jeszcze raz usłyszeć dokładnie tę samą melodię.

## Przydatne Funkcje Randomizacji

Sonic Pi zawiera wiele przydatnych funkcji do działania z losowym strumieniem. Oto lista paru najbardziej użytkowych z nich:

* `rand` - Zwyczajnie zwraca następną wartość w losowym strumieniu
* `rrand` - Zwraca losową wartość z danego zakresu
* `rrand_i` - Zwraca losową liczbę całkowitą z danego zakresu
* `one_in` - Zwraca true lub false z danym prawdopodobieństwem
* `dice` - naśladuje rzut kostki i zwraca wartość między 1 a 6
* `choose` - Wybiera losową wartość z listy

Sprawdź ich dokumentację w systemie pomocy dla uzyskania szczegółowych informacji i przykładów.

## Resetowanie Strumienia

Kiedy zdolność powtarzania sekwencji wybranych nut jest niezbędna, aby pozwolić Ci odtwarzać riff na parkiecie, to nie może być to dokładnie taki riff, jakiego oczekujesz. Czy nie byłoby to wspaniałe, gdybyśmy mogli wypróbować wielu różnych riffów i wybrać taki, który uważamy za najlepszy? Oto miejsce, gdzie zaczyna się prawdziwa magia.

Możemy ręcznie ustawić strumień z fn `use_random_seed`. W informatyce losowy seed to punkt początkowy, z którego nowy strumień składający się z losowych wartości może kiełkować i kwitnąć jak prawdziwy kwiat. Spróbujmy tego:

```
use_random_seed 0
3.times do
  play rrand_i(50, 95)
  sleep 0.125
end
```

Doskonale, mamy pierwsze trzy nuty powyżej z losowej melodii: `84`, `83` oraz `71`. Natomiast możemy zmienić teraz seed na cokolwiek innego. Co powiesz na to:

```
use_random_seed 1
3.times do
  play rrand_i(50, 95)
  sleep 0.125
end
```

Interesujące, mamy teraz `83`, `71` oraz `61`. Możesz zauważyć, że pierwsze dwa numery z tej krótkiej listy są takie same jak przedtem - to nie jest przypadkowy zbieg okoliczności.

Pamiętaj, że strumień losowych wartości jest tak naprawdę po prostu olbrzymią listą 'wylosowanych wcześniej' wartości. Korzystając z losowego ziarna, powoduje, że po prostu przeskakujemy do pewnego punktu w tej liście. Innym sposobem myślenia o tym to próba wyobrażenia sobie ogromnej, potasowanej zawczasu talii kart. Korzystanie z losowego ziarna (`seed`) powoduje odcięcie tej talii od pewnego momentu. Fantastyczną rzeczą jest to, iż pozwala nam to na bardzo precyzyjne skakanie po strumieniu losowych wartości, a co za tym idzie, daje nam ogromne możliwości, gdy tworzymy muzykę.

Powtórzmy naszą losową melodię z 8 nut z nowym strumieniem zresetowanej mocy, ale także dajmy ją w żywą pętlę, co pozwoli nam eksperymentować z kodem na żywo podczas jego odtwarzania:

```
live_loop :random_riff do    
  use_random_seed 0
  8.times do
    play rrand_i(50, 95), release: 0.1
    sleep 0.125
  end
end
```
  
Podczas kiedy nadal jest odtwarzany, zmień wartość seed'u z `0` na cokolwiek innego - masz tutaj wolną rękę. Co powiesz na `100`, a może skusisz się na `999`? Spróbuj swoich własnych wartości, eksperymentuj i baw się z kodem - zobacz, który seed generuje najlepszy Twoim zdaniem riff.

## Połączenie tego wszystkiego w jedną całość

W tym miesiącu poradnik był mocno technicznym zanurzeniem się w to, jak w Sonic Pi działa mechanizm losowości. Mam nadzieję, że pozwoliło Ci to na pewne zaznajomienie się, jak działa ta funkcjonalność oraz że będziesz czuł się komfortowo, aby zacząć tworzyć powtarzalne wzorce w Twojej muzyce. Chciałbym zwrócić Twoją uwagę na to, iż możesz używać powtarzalnej losowości *gdziekolwiek* tylko chcesz. Na przykład możesz losowo różnicować poziom głośności Twoich nut, tempo Twojego rytmu, poziom efektu reverb, aktualny syntezator, poziom opcji mix dla danego efektu itp., itd. W przyszłości pochylimy się nad niektórymi z tych przykładów, ale teraz pozwól, że zostawię Cię z tym krótkim przykładem.

Wpisz następujący kod do czystego bufora, naciśnij Run i następnie zmieniając wartości seed dookoła, naciśnij Run jeszcze raz (kiedy muzyka ciągle jest grana). Odkrywaj różne dźwięki, rytmy i melodie, które możesz stworzyć. Kiedy znajdziesz ten jeden całkiem niezły, zapamiętaj wartość seed, aby móc do niego później powrócić. Gdy na końcu znajdziesz kilka seedów, które Ci się podobają, użyj ich w występie kodowanym na żywo dla Twoich znajomych, przełączając się między Twoimi ulubionymi seedami, by stworzyć cały kawałek.

```
live_loop :random_riff do
  use_random_seed 10300
  use_synth :prophet
  s = [0.125, 0.25, 0.5].choose
  8.times do
    r = [0.125, 0.25, 1, 2].choose
    n = (scale :e3, :minor).choose
    co = rrand(30, 100)
    play n, release: r, cutoff: co
    sleep s
  end
end
live_loop :drums do
  use_random_seed 2001
  16.times do
    r = rrand(0.5, 10)
    sample :drum_bass_hard, rate: r, amp: rand
    sleep 0.125
  end
end
```
