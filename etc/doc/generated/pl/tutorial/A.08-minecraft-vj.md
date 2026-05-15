A.8 Zostań VJ'em Minecraft'a

# Zostań VJ'em Minecraft'a

![Screen 0](../../../etc/doc/images/tutorial/articles/A.08-minecraft-vj/minecraft-vj-0-small.png)


Minecraft Pi


Każdy grał w Minecraft'a i każdy z nas budował niesamowite budowle, projektował pomysłowe pułapki, a nawet stworzył wypracowane linie przewozowe kontrolowane za pomocą czerwonych kamieni pełniących rolę przełączników. Ale jak wielu z was korzystało z Minecraft do koncertowania? Założę się, że żaden z Was nie wiedział, iż można użyć Minecraft'a do stworzenia niesamowitych wizualizacji, tak jak to robią profesjonalni VJ'e.

Jeśli Twoim jedynym sposobem na wprowadzanie zmian w świecie Minecraft była myszka, to masz pewnie za sobą ciężki czas zmieniania rzeczy w wystarczająco szybkim czasie. Na szczęście Twoje Raspberry Pi domyślnie jest wyposażone w wersję Minecraft'a, która może być kontrolowana za pomocą kodu. Ponadto posiada ono domyślnie zainstalowaną aplikację Sonic Pi umożliwiającą nie tylko łatwe kodowanie Minecraft'a ale też i sprawiającą, że jest to niesamowicie fajne.

W dzisiejszym artykule pokażemy Ci parę sztuczek i trików, których używaliśmy do występowania w klubach oraz na wydarzeniach muzycznych na całym świecie.

Zaczynajmy...

## Pierwsze Kroki

Zacznijmy na rozgrzewkę z czymś prostym, aby odświeżyć sobie podstawy. Najpierw włącz Twojego Raspberry Pi, a następnie uruchom Minecraft'a i Sonic Pi. W Minecraft, stwórz nowy świat, a w Sonic Pi wybierz świeży pusty bufor i wprowadź do niego następujący kod:

```
mc_message "Let's get started..."
```
    
Naciśnij przycisk Run, a zobaczysz wiadomość w oknie Minecraft'a. Dobrze, jesteśmy gotowi, by zacząć, zabawmy się...

## Burze Piaskowe

Kiedy używamy Minecraft'a do tworzenia wizualizacji, próbujemy i zastanawiamy się co będzie zarówno wyglądać ciekawie jak i będzie łatwe do wygenerowania za pomocą kodu. Jedną z ciekawych sztuczek jest stworzenie burzy piaskowej poprzez zrzucenie bloków piasku z nieba. Jedyne co do tego jest potrzebne to kilka podstawowych funkcji:

* `sleep` - dla wstawiania przerw pomiędzy akcjami
* `mc_location` - aby odczytać naszą aktualną pozycję
* `mc_set_block`- aby umieścić bloki piasku w określonej lokalizacji
* `rrand` - pozwala nam na wygenerowanie losowych wartość w określonym przedziale
* `live_loop` - pozwala nam na ciągłe tworzenie deszczu piasku

Jeśli którakolwiek z wbudowanych funkcji takich jak np. `rrand` jest dla Ciebie czymś nowym, wystarczy, że wpiszesz te słowo do buforu, ustawisz na nim kursor, a następnie naciśniesz kombinację klawiszy (skrót) `Control-i`, a spowodujesz, że otworzy się wbudowana dokumentacja. Innym sposobem jest przejście na zakładkę *Język* w systemie pomocy (przycisk Help), by następnie poszukać bezpośrednio funkcji, o której chcesz poczytać, podczas gdy w międzyczasie możesz robić wiele innych ekscytujących rzeczy (uwaga od tłumacza: niestety na razie wszystkie funkcje są opisane tylko po angielsku).

Spróbujmy sprawić by deszcz był delikatny, zanim rozpętamy sztorm w pełnej jego mocy. Przechwyć swoją aktualną lokalizację i użyj jej, aby stworzyć kilka bloków piasku gdzieś niedaleko na niebie:

```
x, y, z = mc_location
mc_set_block :sand, x, y + 20, z + 5
sleep 2
mc_set_block :sand, x, y + 20, z + 6
sleep 2
mc_set_block :sand, x, y + 20, z + 7
sleep 2
mc_set_block :sand, x, y + 20, z + 8
```
    
Kiedy naciśniesz przycisk Run, być może będziesz musiał rozejrzeć się trochę dookoła, gdyż bloki mogą zacząć spadać za Tobą, a to zależy, w którym kierunku akurat będziesz zwrócony. Nie przejmuj się, jeśli je przegapiłeś, po prostu uruchom ponownie przycisk Run dla kolejnej partii padającego piasku - upewnij się tylko, że patrzysz we właściwą stronę!

Spróbujmy szybko się rozejrzeć, co tutaj się wyprawia. W pierwszej linii przechwyciliśmy aktualne położenie Steve'a jako koordynaty za pomocą funkcji `mc_location` i zapisaliśmy je w zmiennych `x`, `y` i `z`. Następnie w kolejnych linijkach użyliśmy funkcji `mc_set_block`, aby umieścić trochę piasku w tym samym miejscu, w którym znajduje się Steve, ale z małymi modyfikacjami. Wybraliśmy takie same wartości dla koordynatu x, parametr y ustawiliśmy na 20 bloków wyżej, a na końcu znacznie większą wartość dla koordynatu z, żeby piasek opadał w pewnej odległości od Steve'a.

A może byś tak wziął ten kod i zaczął się nim bawić sam? Spróbuj dodać kolejne linie, zmienić czasy przerw (`sleep`), wymieszać piasek (`:sand`) ze żwirem (`:gravel`) albo wybrać inne współrzędne. Po prostu eksperymentuj i baw się dobrze!

## Żywe Pętle Zdemaskowane

Dobrze, nadszedł czas, aby rozhulać naszą burzę dzięki uwolnieniu pełnej potęgi żywej pętli `live_loop` - magicznej zdolności Sonic Pi, która ujawnia prawdziwą potęgę kodowania na żywo - zmienianie kodu w locie!

```
live_loop :sand_storm do
  x, y, z = mc_location
  xd = rrand(-10, 10)
  zd = rrand(-10, 10)
  co = rrand(70, 130)
  synth :cnoise, attack: 0, release: 0.125, cutoff: co
  mc_set_block :sand, x + xd, y+20, z+zd
  sleep 0.125
end
```
    
Ale zabawa! Kręcimy się w kółko całkiem szybko (8 razy na sekundę) i w trakcie każdego przebiegu odczytujemy aktualną lokację Steve'a tak samo jak wcześniej, ale tym razem generujemy 3 losowe wartości:

* `xd` - różnica dla x, która będzie między -10 a 10
* `zd` - różnica dla z także między -10 a 10
* `co` - wartość odcięcia dla filtra niskich tonów pomiędzy 70 a 130

Następnie używamy tych wartości w fns `synth` oraz `mc_set_block', co daje nam bloki piasku spadające w losowych miejscach wokół Steve'a razem z perkusyjnym dźwiękiem deszczu podobnym do `:cnoise`.

Dla nowych z żywymi pętlami - to jest miejsce, gdzie naprawdę zaczyna się zabawa z Sonic Pi. Spróbuj zmienić wartość uśpienia na `0.25` lub `:sand` na `:gravel:`, podczas spadania bloków piasku wokół Steve'a. Następnie naciśnij *jeszcze raz* przycisk Run. Bułka z masłem! Zmieniło się to bez zatrzymywania uruchomionego kodu! To jest Twoja brama do działania jako prawdziwy VJ. Nie krępuj się zmieniać rzeczy dookoła - jak różne można tworzyć wizualizacje bez zatrzymywania kodu?

## Epickie Wzory Bloków

![Screen 1](../../../etc/doc/images/tutorial/articles/A.08-minecraft-vj/minecraft-vj-1-small.png)

Na końcu innym bardzo dobrym sposobem na generowanie ciekawych wizualizacji jest generowanie ogromnych ozdobionych wzorami ścian i przemieszczanie się w ich kierunku oraz blisko nich. Aby uzyskać taki efekt, musimy zmienić podejście i, zamiast losowego ustawiania bloków, zacząć je układać w sposób uporządkowany. Możemy tego dokonać poprzez zagnieżdżenie dwóch pętli (naciśnij przycisk Help i przejdź w poradniku do sekcji 5.2 zatytułowanej "Iteracja i Pętle", aby dowiedzieć się więcej o iteracjach). Zabawne polecenie `|xd|` tuż po poleceniu `do` oznacza, że przy każdej iteracji do zmiennej `xd` zostanie przypisana wartość, po której aktualnie iterujemy. Będą to więc kolejno 0, potem 1, potem 2, ...., itd. Dzięki podwójnemu zagnieżdżeniu dużej ilości iteracji, tak jak to robimy tutaj, możemy wygenerować wszystkie współrzędne do narysowania kwadratu. Następnie mamy możliwość losowo wybierać typ bloku dla uzyskania ciekawego efektu:

```
x, y, z = mc_location
bs = (ring :gold, :diamond, :glass)
10.times do |xd|
  10.times do |yd|
    mc_set_block bs.choose, x + xd, y + yd, z
  end
end
```

Całkiem schludnie. Kiedy czerpiemy radość z zabawy, spróbuj zmienić `bs.choose` na `bs.tick`, aby ruszyć z losowego wzoru na bardziej regularny. Zrób to także z typami bloków. Niektórzy z Was będą pewnie chcieli wypróbować to w taki sposób, żeby zmieniały się one automatycznie.

Czas na finał VJ - zmień dwa `10.times` na `100.times` oraz kliknij *Run*. Bum! Ogromna gigantyczna ściana zbudowana z losowo rozmieszczonych cegieł. Wyobraź sobie, ile czasu zajęłoby zbudowanie tego za pomocą myszki! Naciśnij dwa razy spację w krótkim odstępie czasu, by wznieść się w powietrze, a następnie zacznij pikować dla paru wspaniałych efektów wizualnych. Nie zatrzymuj tutaj swoich myśli - użyj wyobraźni do wyczarowania naprawdę fajnych pomysłów, a następnie skorzystaj z mocy kodowania Sonic Pi do uczynienia ich prawdziwymi. Kiedy potrenujesz wystarczającą ilość czasu, przyciemnij światło i stwórz pokaz VJ dla Twoich znajomych!
