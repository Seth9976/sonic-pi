A.6 Muzyczny Minecraft

# Muzyczny Minecraft


Minecraft Pi


Witam ponownie! W poprzednich tutorialach skupiliśmy się tylko i wyłącznie na muzycznych możliwościach Sonic Pi - zmieniając w ten sposób Twój komputer (np. Raspberry Pi) w gotowy do wystąpień publicznych instrument muzyczny. Do tej pory opanowaliśmy następujące umiejętności:

* Kodowania Na Żywo - zmienianie dźwięków w locie
* Kodowania paru potężnych bitów,
* Wygenerowania potężnych melodii uzyskanych przy pomocy syntezatorów,
* Odtworzyliśmy sławne brzmienie acid'owego basu TB-303.

Istnieje znacznie więcej rzeczy, które mógłbym Ci pokazać (i omówimy je w kolejnych edycjach). Jednakże w tym miesiącu spójrzmy na coś, co Sonic Pi potrafi, a Ty prawdopodobnie nawet sobie nie zdawałeś z tego sprawy: kontrolowanie Minecrafta.

## Witaj Świecie Minecrafta

OK, zaczynajmy! Włącz Twojego Raspberry Pi, uruchom Minecraft Pi i stwórz nowy świat. Teraz uruchom program i zmień rozmiar oraz ustawienie okien w taki sposób, abyś mógł jednocześnie widzieć Sonic Pi i Minecraft Pi.

W czystym buforze wpisz następujący kod:

```
mc_message "Hello Minecraft from Sonic Pi!"
```
    
A teraz naciśnij przycisk Run. Bum! Twoja wiadomość pojawiła się w Minecraft! Jakie to proste! A teraz przerwij na chwilę czytanie i spróbuj stworzyć swoje własne wiadomości. Milej zabawy!

![Screen 0](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-0-small.png)

## Teleporter Dźwiękowy

Czas na małe rozpoznanie. Sięgnięcie po myszkę i klawiaturę jest standardową opcją, która umożliwia chodzenie po naszym świecie. To działa, ale jest całkiem wolne i nudne. Byłoby znacznie lepiej, gdybyśmy mieli pewien rodzaj maszyny umożliwiającej teleportację. Dzięki Sonic Pi mamy takie cudo. Spróbuj tego:

```
mc_teleport 80, 40, 100
```
    
Niespodzianka! To była długa droga w górę. Jeśli nie byłeś akurat w trybie latania, wtedy prawdopodobnie spadłeś w dół aż do samej ziemi. Jeśli szybko naciśniesz 2 razy spację, aby wejść w tryb latania i spróbujesz jeszcze raz się przeteleportować, zaczniesz unosić się w miejscu, do którego się przeniosłeś.

Chyba czas na wyjaśnienie, co oznaczają te liczby. Mamy trzy cyfry opisujące koordynaty miejsca w naszym świecie, do którego chcemy się przenieść. Nadamy każdej z liczb nazwę - x, y i z:

* x - jak daleko w lewo lub prawo (w naszym przykładzie jest to 80)
* y - jak wysoko chcemy się znaleźć (w naszym przykładzie jest to 40)
* z - jak daleko w przód lub do tyłu (w naszym przykładzie 100)

Wybierając różne wartości dla x, y i z, możemy teleportować się *gdziekolwiek* w naszym świecie. Spróbuj! Wybierz inne liczby i zobacz, gdzie możesz wylądować. Jeśli ekran stanie się czarny, będzie tak, ponieważ przeteleportowałeś się pod ziemię lub w głąb góry. Wystarczy, że wybierzesz po prostu większą wartość y, aby ponownie znaleźć się powyżej gruntu. Eksploruj teren tak długo, aż znajdziesz się w miejscu, które Ci się podoba...

Korzystając z dotychczasowych idei, zbudujmy Dźwiękowy (Sonic) Teleporter, który będzie wydawał fajne dźwięki teleportacji w momencie, gdy będziemy śmigać wskroś świata Minecraft:

```
mc_message "Preparing to teleport...."
sample :ambi_lunar_land, rate: -1
sleep 1
mc_message "3"
sleep 1
mc_message "2"
sleep 1
mc_message "1"
sleep 1
mc_teleport 90, 20, 10
mc_message "Whoooosh!"
```
    
![Screen 1](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-1-small.png)

## Magiczne Bloki

Teraz, gdy już znalazłeś fajne miejsce, zacznijmy budować. Mógłbyś zrobić to, do czego przywykłeś i zacząć wściekle klikać myszką, aby zacząć umieszczać kolejne bloki pod kursorem. Możesz też użyć magii Sonic Pi. Spróbuj tego:

```
x, y, z = mc_location
mc_set_block :melon, x, y + 5, z
```

A teraz spójrz! Na niebie pojawił się melon. Poświęć chwilę, aby przyjrzeć się temu kawałkowi kodu. Co zrobiliśmy? W linii pierwszej przechwyciliśmy aktualne położenie Steve'a jako zmienne x, y i z. Odpowiada to naszym koordynatom, które omówiliśmy już wcześniej. Następnie używamy tych współrzędnych w funkcji `mc_set_block`, co powoduje umieszczenie wybranego bloku w określonych współrzędnych. Aby zrobić coś wyżej na niebie, wystarczy, że zwiększymy wartość y. I to jest właśnie przyczyna, dla której dodajemy 5. Spróbujmy utworzyć z nich długą ścieżkę:

```
live_loop :melon_trail do
  x, y, z = mc_location
  mc_set_block :melon, x, y-1, z
  sleep 0.125
end
```

A teraz przenieś się do Minecraft, upewnij, że jesteś w trybie latania (wciśnij dwa razy spację jeśli nie) i przeleć dookoła po świecie. Spójrz za siebie, aby zobaczyć za sobą ścieżką złożoną z melonowych bloków! Zobacz, jaki rodzaj pokręconych wzorków możesz zrobić na niebie.

## Kodowanie Na Żywo z Minecraft

Ci Was, którzy obserwowali ten poradnik przez ostatnich kilka miesięcy, prawdopodobnie czują teraz, że ich umysł za chwilę eksploduje. Ścieżka z melonów wygląda co prawda całkiem ciekawie, ale najbardziej ekscytującą częścią poprzedniego przykładu jest to, że możesz użyć w Minecraft żywej pętli `live_loop`! Dla tych, co jeszcze nie wiedzą - polecenie `live_loop` daje Sonic Pi specjalną magiczną zdolność, a nie posiada jej żaden inny język programowania. Pozwala Ci na uruchomienie wielu różnych pętli w tym samym czasie i zmianę ich zachowania w trakcie działania. Dzięki temu są one niewiarygodnie potężne i niesamowicie fantastyczne. Używam polecenia `live_loop` dostępnego w Sonic Pi do koncertowania na żywo w klubach - DJ'e korzystają z płyt, a ja z żywych pętli (`live_loop`'y). Dzisiaj będziemy jednak kodować na żywo zarówno muzykę, jak i Minecraft.

Zaczynajmy. Uruchom powyższy kod i zacznij ponownie kreślić swoją ścieżkę z melonów. Teraz bez zatrzymywania kodu, po prostu zmień polecenie `:melon` na `:brick` (cegła) i naciśnij przycisk Run. Hej, zobacz, jak szybko robisz teraz ścianę z cegieł. Jakież to było proste! Wyobrażasz sobie może jakąś melodię, która mogłaby być odtwarzana, gdy to się dzieje? Bułka z masłem! Spróbuj tego:

```
live_loop :bass_trail do
  tick
  x, y, z = mc_location
  b = (ring :melon, :brick, :glass).look
  mc_set_block b, x, y -1, z
  note = (ring :e1, :e2, :e3).look
  use_synth :tb303
  play note, release: 0.1, cutoff: 70
  sleep 0.125
end
```
    
Teraz, podczas gdy wciąż gra muzyka zacznij zmieniać kod. Zmień typy bloku - spróbuj `:water` (woda), `:grass` (trawa) albo jakiś inny ulubiony typ bloku. Spróbuj również zmienić wartość odcięcia (cutoff) z `70` na `80` a później na `100`. Czyż to nie jest fajne?

## Połączenie tego wszystkiego w jedną całość

![Screen 2](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-2-small.png)

Spróbujmy połączyć wszystko, co do tej pory widzieliśmy z odrobiną dodatkowej magii. Połączmy naszą zdolność teleportacji z umieszczaniem bloków i muzyki, aby stworzyć Muzyczne Minecraft'owe Video. Nie przejmuj się, jeśli nie wszystko jeszcze rozumiesz, po prostu wpisz ten kawałek i baw się, zmieniając różne wartości, gdy kod jest wciąż uruchomiony. Miłej zabawy i do zobaczenia następnym razem...
    
```
live_loop :note_blocks do
  mc_message "This is Sonic Minecraft"
  with_fx :reverb do
    with_fx :echo, phase: 0.125, reps: 32 do
      tick
      x = (range 30, 90, step: 0.1).look
      y = 20
      z = -10
      mc_teleport x, y, z
      ns = (scale :e3, :minor_pentatonic)
      n = ns.shuffle.choose
      bs = (knit :glass, 3, :sand, 1)
      b = bs.look
      synth :beep, note: n, release: 0.1
      mc_set_block b, x+20, n-60+y, z+10
      mc_set_block b, x+20, n-60+y, z-10
      sleep 0.25
    end
  end
end
live_loop :beats do
  sample :bd_haus, cutoff: 100
  sleep 0.5
end
```
